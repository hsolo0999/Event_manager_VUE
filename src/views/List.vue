<template>
  <div>
    <h3>Список задач</h3>

    <div class="row">
      <div class="input-field col s6 ">
        <select ref="select" v-model="filter">
          <option value="" disabled selected>Выберите фильтр</option>
          <option value="active">Активные</option>
          <option value="outdated">Просроченные</option>
          <option value="completed">Выполненные</option>
        </select>
        <label>Фильтр</label>
      </div>
    </div>

    <button v-if="filter" class="btn btn-small red" @click="filter = null">Сбросить фильтр</button>

    <hr>

    <table v-if="tasks.length">
      <thead>
      <tr>
        <th>#</th>
        <th>Название</th>
        <th>Дедлайн</th>
        <th>Описание</th>
        <th>Статус</th>
        <th></th>
      </tr>
      </thead>
      <tbody>
      <tr
          v-for="(task, idx) of displayTasks"
          :key="task.id"
      >
        <td>{{idx + 1}}</td>
        <td>{{task.title}}</td>
        <td>{{new Date(task.date).toLocaleDateString()}}</td>
        <td class="td">
          <div class="text">{{task.description}}</div>
        </td>
        <td>{{task.status}}</td>
        <td>
          <router-link tag="button" class="btn btn-small" :to="'/task/' + task.id">
            Редактировать
          </router-link>
        </td>
      </tr>
      </tbody>
    </table>
    <p v-else>Вы не создали ни одной задачи</p>
  </div>
</template>

<script>
export default {
  data: () => ({
    filter: null,
  }),
  computed: {
    tasks() {
      return this.$store.getters.tasks
    },
    displayTasks() {
      return this.tasks.filter(t => {
        if (!this.filter) {
          return true
        }
        return t.status === this.filter
      })
    }
  },
  mounted() {
    M.FormSelect.init(this.$refs.select)
  }
}
</script>

<style lang="scss" scoped>
  .td {
    max-width: 400px;
  }

  .text {
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
  }
</style>