<template>
  <div class="col-lg-12 col-md-12 col-xs-12 table-responsive">
    <h2>{{ title }}</h2>
    <table class="table table-hover table-bordered">
      <thead>
        <tr v-for="(data, index) in datas" v-bind:key="index">
          <td>{{ data.id }}</td>
          <td>{{ data.name }}</td>
          <td>{{ data.email }}</td>
          <td>{{ data.body }}</td>
        </tr>
      </thead>
      <tbody id="myTable"></tbody>
    </table>
    <ul class="pagination">
      <li class="pagination-item">
        <button
          type="button"
          @click="onClickFirstPage"
          :disabled="isInFirstPage"
        >
          First
        </button>
      </li>

      <li class="pagination-item">
        <button
          type="button"
          @click="onClickPreviousPage"
          :disabled="isInFirstPage"
        >
          Previous
        </button>
      </li>

      <li v-for="page in pages" class="pagination-item" v-bind:key="page">
        <button
          type="button"
          @click="onClickPage(page.name)"
          :disabled="page.isDisabled"
          :class="{ active: isPageActive(page.name) }"
        >
          {{ page.name }}
        </button>
      </li>

      <li class="pagination-item">
        <button type="button" @click="onClickNextPage" :disabled="isInLastPage">
          Next
        </button>
      </li>

      <li class="pagination-item">
        <button type="button" @click="onClickLastPage" :disabled="isInLastPage">
          Last
        </button>
      </li>
    </ul>
    <pagination
      :total-pages="11"
      :total="113"
      :per-page="10"
      :current-page="currentPage"
      @pagechanged="onPageChange"
    />
    <!-- <paginate
      :page-count="20"
      :page-range="3"
      :margin-pages="2"
      :click-handler="clickCallback"
      :prev-text="'Prev'"
      :next-text="'Next'"
      :container-class="'pagination'"
      :page-class="'page-item'"
    >
    </paginate> -->
  </div>
</template>
<script>
export default {
  props: {
    maxVisibleButtons: {
      type: Number,
      required: false,
      default: 3,
    },
    totalPages: {
      type: Number,
      required: true,
    },
    total: {
      type: Number,
      required: true,
    },
    currentPage: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      title: "Danh sách Comments",
      datas: [],
    };
  },
  created: function () {
    this.danhsach_comments();
  },
  methods: {
    danhsach_comments() {
      this.axios
        .get("http://jsonplaceholder.typicode.com/comments")
        .then((response) => {
          this.datas = response.data;
        });
    },
    onClickFirstPage() {
      this.$emit("pagechanged", 1);
    },
    onClickPreviousPage() {
      this.$emit("pagechanged", this.currentPage - 1);
    },
    onClickPage(page) {
      this.$emit("pagechanged", page);
    },
    onClickNextPage() {
      this.$emit("pagechanged", this.currentPage + 1);
    },
    onClickLastPage() {
      this.$emit("pagechanged", this.totalPages);
    },
    isPageActive(page) {
      return this.currentPage === page;
    },
  },
  computed: {
    startPage() {
      // When on the first page
      if (this.currentPage === 1) {
        return 1;
      }
      // When on the last page
      if (this.currentPage === this.totalPages) {
        return this.totalPages - this.maxVisibleButtons;
      }
      // When in between
      return this.currentPage - 1;
    },
    pages() {
      const range = [];

      for (
        let i = this.startPage;
        i <=
        Math.min(this.startPage + this.maxVisibleButtons - 1, this.totalPages);
        i += 1
      ) {
        range.push({
          name: i,
          isDisabled: i === this.currentPage,
        });
      }

      return range;
    },
    isInFirstPage() {
      return this.currentPage === 1;
    },
    isInLastPage() {
      return this.currentPage === this.totalPages;
    },
  },
};
</script>
 <style>
table {
  right: 0;
  left: 0;
  top: 0;
  margin: auto;
}
table tr th {
  background: rgba(0, 145, 234, 1);
  padding: 10px;
  color: #fff;
}
table tr td {
  padding: 10px;
  border-right: 1px solid rgba(0, 0, 0, 0.1);
  font-size: 15px;
}
.pagination {
  list-style-type: none;
}

.pagination-item {
  display: inline-block;
}

.active {
  background-color: #4aae9b;
  color: #ffffff;
}
</style>
