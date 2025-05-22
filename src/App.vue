<template>
  <div class="main">

    <div class="bg">
      <div class="ls">
        <div class="overlay"></div>
      </div>
    </div>

    <div class="content">

      <div class="input" @keyup.enter="addData">
        <input v-model="newData" placeholder="Tulis sesuatu..." />
        <button @click="addData" class="btn-icon">
          <img src="/add.png" alt="">
        </button>
      </div>

      <div class="index">
        <transition-group name="list" tag="ul">
          <li v-for="item in dataList" :key="item.id">
            <div class="item">
              <label class="custom-checkbox left-side">
                <input type="checkbox" v-model="item.done" />
                <span class="checkmark"></span>
                <span :class="{ done: item.done }">{{ item.text }}</span>
              </label>
              <button class="delete-btn" @click="deleteData(item.id)">✖</button>
            </div>
          </li>
        </transition-group>
      </div>

      <div class="status">
        <div class="left-status">
          <span>Belum: {{ totalUndone }}</span>
          <span> | </span>
          <span>Sudah: {{ totalDone }}</span>
        </div>
        <div class="right-status">
          <button @click="clearAll" class="clear-btn">Clear All</button>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newData: '',
      dataList: [],
      nextId: 1, // buat generate id unik
    };
  },
  computed: {
    totalDone() {
      return this.dataList.filter(item => item.done).length;
    },
    totalUndone() {
      return this.dataList.filter(item => !item.done).length;
    }
  },
  methods: {
    addData() {
      if (this.newData.trim() !== '') {
        this.dataList.push({
          id: this.nextId++,  // kasih id unik
          text: this.newData,
          done: false,
        });
        this.newData = '';
      }
    },
    deleteData(id) {
      this.dataList = this.dataList.filter(item => item.id !== id);
    },
    clearAll() {
      this.dataList = [];
    }
  }
};
</script>
