<script>
import axios from 'axios'
import LazyList from 'lazy-load-list/vue'
import colors from './colors'
import resultProducts from './resultProducts'

export default {
  components: {
    LazyList
  },
  data() {
    return {
      param: '',
      api: 'https://jsonplaceholder.typicode.com/',
      resultProducts,
      colors,
      resultCheck: false,
      sortBy: 'id',
      sortDirection: 'asc',
    }
  },
  methods: {
    async getProducts() {
      if (this.param) {
        this.resultCheck = false

        await axios.get(this.api + this.param)
          .then(response => {
            this.resultProducts = response.data
          }
          )
        this.resultCheck = true
        this.param = ''
      }
    },
    sort: function (s) {
      if (s === 'asc') {
        this.sortDirection = 'asc';
      } else {
        this.sortDirection = 'desc';
      }
    }
    // xY() {
    //   this.tmp = this.resultProducts
    // }
  },
  computed: {
    sortedProducts: function () {
      return this.resultProducts.sort((p1, p2) => {
        let modifier = 1;
        if (this.sortDirection === 'desc') modifier = -1;
        if (p1[this.sortBy] < p2[this.sortBy]) return -1 * modifier;
        if (p1[this.sortBy] > p2[this.sortBy]) return 1 * modifier;
        return 0;
      });
    }
  },
}
</script>

<template>
  <div class="sidebar bg-white">
    <div class="py-2 text-center filter fw-bold rounded ">
      <i class="fa fa-filter me-2"></i>Bộ lọc tìm kiếm
    </div>
    <h5 class="mt-4">Theo shop</h5>
    <form>
      <ul class="">
        <li>
          <input class="form-check-input" type="checkbox" id="gearvn">
          <label class="form-check-label ms-2" for="gearvn">Gear VN</label>
        </li>
        <li>
          <input class="form-check-input" type="checkbox" id="tiki">
          <label class="form-check-label ms-2" for="tiki">Tiki</label>
        </li>
        <li>
          <input class="form-check-input" type="checkbox" id="lazada">
          <label class="form-check-label ms-2" for="lazada">Lazada</label>
        </li>
        <li>
          <input class="form-check-input" type="checkbox" id="shopee">
          <label class="form-check-label ms-2" for="shopee">Shopee</label>
        </li>
      </ul>
    </form>
  </div>

  <div class="main">
    <div class="navbar fixed-top bg-white border-bottom pb-3">
      <input @keyup.enter="getProducts" v-model="param" class="search-box" type="text" placeholder="Search.."
        name="search">
      <button @click="getProducts" class="btn-search"><i class="fa fa-search"></i></button>
      <button @click="sort('desc')" class="btn-sort down">
        <i class="fa fa-caret-down"></i>
        Giá cao
      </button>
      <button @click="sort('asc')" class="btn-sort up">
        <i class="fa fa-caret-up"></i>
        Giá thấp
      </button>
      <div class="ms-auto">
        <label class="">Khoảng giá</label>
        <input class="range range-down" type="number" placeholder="Tối thiểu">
        -
        <input class="range range-up ms-0" type="number" placeholder="Tối đa">
        <button class="btn-range fw-bold" type="submit">Áp dụng</button>
      </div>
    </div>

    <div class="x" v-if="resultCheck">
      <LazyList :data="sortedProducts" :itemsPerRender="20" containerClasses="list" defaultLoadingColor="false">
        <template v-slot="{ item }">
          <a class="item-card" :href="item.thumbnailUrl" target="_blank">
            <img :src="item.thumbnailUrl">
            <div>{{ item.title }}</div>
            <span>{{ item.id }}</span>
            <span class="float-end">{{ item.albumId }}</span>
          </a>
        </template>
      </LazyList>
    </div>


    <!-- <a class="item-card" v-for="item in resultProducts" :key="item.id" :href="item.thumbnailUrl" target="_blank">
      <img :src="item.thumbnailUrl">
      <div>{{ item.title }}</div>
      <span>{{ item.id }}</span>
      <span class="float-end">{{ item.albumId }}</span>
    </a> -->
    <!-- <a class="item-card" v-for="item in resultProducts" :key="item.image" :href="item.image" target="_blank">
      <img :src="urlimg + item.image" >
      <div>{{ item.name }}</div>
      <span>{{ item.price }}</span>
      <span class="float-end">{{ item.price }}</span>
    </a> -->

  </div>
  <!-- <p>{{ resultProducts }}</p>
  <p>{{colors}}</p> -->
  <p>{{ tmp }}</p>
</template>

<style>
* {
  box-sizing: border-box;
}

.sidebar {
  height: 100%;
  width: 300px;
  position: fixed;
  z-index: 1;
  top: 0;
  left: 0;
  overflow-x: hidden;
  padding: 20px 10px 0px;
  border-right: 1px solid #ccc;
  /* box-shadow: 3px 0px 10px rgba(0, 0, 0, 0.19); */
}

.filter {
  background-color: #ffd154;
  color: #002795;
}

.main {
  margin-left: 300px;
  /* font-size: 28px; */
  /* padding-left: 10px; */
  padding-top: 80px;
}

.search-box {
  width: 350px;
  padding: 6px;
  font-size: 17px;
  border: none;
  border-bottom: 1px solid #ddd;
}

.btn-search {
  padding: 6px 10px;
  /* margin-top: 8px; */
  /* margin-right: 16px; */
  background: #ddd;
  font-size: 18px;
  border: none;
  cursor: pointer;
}

.btn-search:hover {
  background: #ccc;
}

.btn-sort {
  border: none;
  border-radius: 5px;
  padding: 8px 20px;
  margin-left: 10px;
}

.btn-sort i {
  margin-right: 8px;
}

.up {
  background: #46c546;
  color: white;
}

.up:hover {
  background: #3dae3d;
}

.down {
  background-color: #0063ec;
  color: white;
}

.down:hover {
  background-color: #0357cd;
}

.range {
  margin-left: 5px;
  width: 100px;
  padding: 6px;
  font-size: 14px;
  border-radius: 3px;
  border: 1px solid #ccc;
}

/* .range-down {
  margin-left: 20px;
}

.range-up {
  margin-left: 5px
} */

.btn-range {
  border: none;
  border-radius: 5px;
  padding: 8px 20px;
  margin-left: 5px;
  background-color: #ffd154;
  color: #002795;
}

.btn-range:hover {
  background-color: #e7bd4a;
}

/* .range-box {
  margin-left: 100px;
} */

.fixed-top {
  top: 0;
  right: 0;
  left: 299px;
  padding-top: 20px;
  padding-right: 20px;
  padding-left: 20px;
}

ul {
  list-style-type: none;
}

img {
  width: 250px;
  height: 220px;
}

.item-card {
  height: 320px;
  width: 251px;
  position: relative;
  overflow: hidden;
  margin: 10px 20px;
  /* border: 1px solid #ccc; */
  box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.2);

}

.item-card:hover {
  box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.4);
  color: unset;
}

a {
  text-decoration: none;
  color: unset;
}

.x {
  width: 1230px;
  height: 630px;
  overflow: hidden;
}

.list {
  justify-content: center;
  display: flex;
  flex-wrap: wrap;
  padding: 24px;
}
</style>