<template>
  <div>
    <div class="container" id="todo">
      <section class="panel">
        <input type="checkbox" id="mark-all" @click="selectAll" :checked="areAllSelected">
        <b-form-checkbox
          v-b-tooltip.hover.left="'Finalizeaza toate task-urile!'"
          type="checkbox"
          id="mark-all"
          @click="selectAll"
          :checked="areAllSelected"
          switch
        ></b-form-checkbox>
        <input
          v-model="newTask"
          @keyup.enter="addTask"
          placeholder="Ce vrei sa adaugi pe lista ?"
          class="text-input"
        >
        <b-button @click="addTask" variant="info">
          <i class="fa fa-plus"></i>
        </b-button>
        <b-button
          v-b-tooltip.hover
          title="Sterge toate task-urile!"
          @click="clearList"
          block
          variant="danger"
        >Sterge lista</b-button>
      </section>

      <section class="list">
        <ul class="list-item">
          <li v-for="task in tasks" :class="{done: isChecked(task)}">
            <input type="checkbox" class="checkbox" @click="check" v-model="task.checked">
            <b-form-checkbox
              v-b-tooltip.hover.left="'Bifeaza ca finalizat!'"
              type="checkbox"
              class="checkbox"
              @click="check"
              v-model="task.checked"
              switch
            ></b-form-checkbox>
            <input
              type="text"
              v-if="task === editingTask"
              class="text-input"
              @keyup.enter="endEditing(task)"
              @blur="endEditing(task)"
              v-model="task.text"
            >

            <label
              for="checkbox"
              v-if="task !== editingTask"
              @dblclick="editTask(task)"
            >{{ task.text }}</label>

            <button v-if="task !== editingTask" @click="editTask(task)">
              <i class="fa fa-edit" v-b-tooltip.hover.lefttop="'Editeaza task!'"></i>
            </button>
            <button v-if="task === editingTask" @click="endEditing(task)">
              <i class="fa fa-plus"></i>
            </button>

            <button
              v-b-popover="'Pentru a edita un task apasa dublu click pe task sau iconita de editare!'"
              title="Cum editezi un task?"
            >
              <i class="fa fa-info"></i>
            </button>
            <button class="delete" @click="removeTask(task)">
              <i class="fa fa-trash" v-b-tooltip.hover.righttop="'Sterge task!'"></i>
            </button>
          </li>
        </ul>
      </section>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: "",
      tasks: [
        {
          text: "Task exemplu. Adauga task-urile tale mai sus!",
          checked: false
        }
      ],

      editingTask: {}
    };
  },

  computed: {
    areAllSelected: function() {
      return (
        this.tasks.every(function(task) {
          return task.checked;
        }) && this.tasks.length > 0
      );
    }
  },

  methods: {
    addTask: function() {
      var task = this.newTask.trim();
      if (task) {
        this.tasks.push({ text: task, checked: false });
        this.newTask = "";
      }
    },

    removeTask: function(task) {
      var index = this.tasks.indexOf(task);
      this.tasks.splice(index, 1);
    },

    editTask: function(task) {
      this.editingTask = task;
    },

    endEditing: function(task) {
      this.editingTask = {};
      if (task.text.trim() === "") {
        this.removeTask(task);
      }
    },

    clearList: function() {
      if (this.tasks.length !== 0) {
        swal({
          title: "Stergi toate task-urile?",
          text: "Task-urile sterge nu mai pot fi recuperate!",
          icon: "warning",
          buttons: true,
          dangerMode: true
        }).then(willDelete => {
          if (willDelete) {
            this.tasks = [];
            swal("Task-urile tale au fost sterse cu succes!", {
              icon: "success"
            });
          } else {
            swal("Task-urile tale sunt in siguranta!");
          }
        });
      } else {
        swal(
          "Momentan nu ai task-uri!",
          "Adauga task-uri pentru a putea utiliza stergerea multipla!",
          "info"
        );
      }
    },

    selectAll: function(task) {
      var targetValue = this.areAllSelected ? false : true;
      for (var i = 0; i < this.tasks.length; i++) {
        this.tasks[i].checked = targetValue;
      }
    },

    check: function(task) {
      task.checked = true;
    },

    isChecked: function(task) {
      return task.checked;
    }
  }
};
</script>

<style lang="scss" scoped>
ul,
li {
  margin: 0;
  padding: 0;
  border: 0;
}

body {
  line-height: 1;
  font-family: "Lato", sans-serif;
  background-color: #eff1f2;
}

.container {
  width: 70%;
  margin: 1em auto 3em;
  border: 1px solid #efefef;
}

.panel,
li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  list-style-type: none;
  padding: 10px;
  border-bottom: 1px solid #efefef;
  background-color: #e7e8eb;
}

.text-input {
  border: 1px solid #dedede;
  padding-left: 10px;
  width: 70%;
  height: 35px;
  color: #555;
}

button {
  color: #555;
  background-color: #ffffff;
  border: 1px solid #bbb;
  outline: none !important;
  width: 100px;
  height: 38px;
  cursor: pointer;
  font-size: 14px;
}

/* Task  area */

.list li {
  background-color: #529a7a;
}

.list li button {
  background-color: transparent;
  border: 1px solid #3465a4;
  color: #ddd;
  visibility: hidden;
  font-size: 20px;
  font-weight: bold;
}

.list li:hover > button {
  visibility: visible;
}

.list label {
  padding-right: 10px;
  display: inline-block;
  width: 70%;
  font-size: 18px;
  line-height: 24px;
  color: #fcfcfc;
  z-index: 2;
  overflow: hidden;
}

.list li.done label {
  color: #d9d9d9;
  text-decoration: line-through;
}

.delete i {
  color: #fff;
}

.list li button[data-v-ee48fd14] {
  border: none !important;
}

.hidden {
  display: none;
}

input[type="checkbox"] {
  display: none;
}

.container[data-v-ee48fd14] {
  border: 0 !important;
}

/* Media Queries */

@media screen and (max-width: 768px) {
  .container {
    width: 90%;
    max-width: 90%;
  }

  .text-input,
  .list label {
    width: 60%;
    font-size: 14px;
  }

  button {
    width: 50px;
    height: 50px;
    outline: none;
  }
}
</style>