<template>
  <div class="container">
    <div class="row">
      <div class="pagination justify-content-center gap" style="margin-top: 50px">
        <div>
          My Paginator gets Three Props
          <ul>
            <li>Data: To get length to calculate total pages that will appear</li>
            <li>
              Number Of Pagination: This is the number of pages that you can navigate
              between
            </li>
            <li>
              Number Of Rows: This is the number of items that will appear on a page
            </li>
          </ul>
          And has one Emit
          <ul>
            <li>
              Update Slice: This updates the start point and end point for items on the
              page depending on how much you navigate
            </li>
          </ul>
        </div>
      </div>
    </div>
    <div class="row">
        <p><b>Example One :</b></p>
        <p>
          Num of Pagination : <b>{{ numOfPagination }}</b>
        </p>
        <p>
          Num of items per page : <b>{{ numOfRows }}</b>
        </p>
        <pagination
          :data="data"
          :numOfPagination="numOfPagination"
          :numOfRows="numOfRows"
          @updateSlice="updateSlice"
        />
    </div>
    <div class="row">
        <p><b>Example Two :</b></p>
        <p>Num of Pagination : <b>8</b></p>
        <p>Num of items per page : <b>50</b></p>
        <pagination
          :data="data"
          :numOfPagination="9"
          :numOfRows="50"
          @updateSlice="updateSlice"
        />
      </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";
import axios from "axios";
import pagination from "@/components/pagination.vue";

const cars = ref([]);
const carSchema = ref(["id", "car_make", "car_model_year", "car_vin"]);
const employees = ref([]);
const empSchema = ref(["id", "first_name", "last_name", "email", "gender", "ip_address"]);
const data = ref([]);
const schema = ref(empSchema); // Use empSchema directly
const first = ref(0);
const numOfRows = ref(20);
const end = ref(numOfRows.value);
const numOfPagination = ref(15);

const getData = async () => {
  try {
    const carRes = await axios.get("/cars.json");
    cars.value = carRes.data;
    const employeesRes = await axios.get("/employees.json");
    employees.value = employeesRes.data;
    data.value = employees.value;
  } catch (err) {
    console.log(err);
  }
};

const updateSlice = ({ first: newFirst, end: newEnd }) => {
  first.value = newFirst;
  end.value = newEnd;
};

onMounted(async () => {
  await getData();
});
</script>

<style scoped>
p {
  background-color: lightblue;
  padding: 15px;
  margin: 15px;
}
</style>
