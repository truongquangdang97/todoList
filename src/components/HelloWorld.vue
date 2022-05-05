<template>
  <div class="hello">
    <section class="wrapper">
      <header><h1>ToDoList</h1></header>
      <div class="ToDo">
        <input
          type="text"
          id="task-input"
          placeholder="what needs to be done?"
          class="add"
          v-model="job"
        />
        <button type="submit" value="+" class="add-btn" @click="addItem()">
          +
        </button>
        <ul class="task-list">
          <template v-for="item in todolist">
            <li class="" :key="item.id">
              <input
                type="checkbox"
                class="toggle"
                @click="checkBox(item)"
              /><span
                id="task-1"
                contenteditable="false"
                spellcheck="false"
                class="task-title"
                :style="
                  item.action === true
                    ? 'text-decoration-line:line-through'
                    : 'text-decoration-line:none'
                "
                >{{ item.nameJob }}</span
              >
              <a href="#" style="float: right" @click="deleteItem(item.id)"
                >delete</a
              >
              <a
                href="#"
                style="float: right; margin-right: 10px"
                @click="editItem(item)"
                >edit</a
              >
            </li>
          </template>
        </ul>
        <div class="bottom">
          <div class="progress-bar" style="display: block">
            <div
              class="progress-status"
              :style="{ width: widthChange, backgroundColor: background }"
            >
              <p>
                <span class="tasks-left">{{ this.count }}</span> of
                <span class="tasks-total">{{ todolist.length }}</span> tasks
                done
              </p>
            </div>
          </div>
          <a
            href="#"
            class="remove-checked"
            :style="{ display: clear }"
            @click="removeAll()"
            ><span>Remove checked</span></a
          >
        </div>
        <div style="color: red">
          Số công việc đã hoàn thành : <b>{{ this.percent }}%</b>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },

  watch: {
    count() {
      if (this.length === 0) {
        return (this.percent = 0);
      }
      return (this.percent = (this.count / this.length) * 100);
    },
    length() {
      if (this.length === 0) {
        return (this.percent = 0);
      }
      return (this.percent = (this.count / this.length) * 100);
    },
  },

  computed: {
    length() {
      return this.todolist.length;
    },
    widthChange() {
      if (this.length === 0) {
        return null;
      }
      return this.percent + "%";
    },
    background() {
      if (0 < this.percent && this.percent < 30) {
        return "red";
      } else if (31 < this.percent && this.percent < 60) {
        return "yellow";
      } else if (61 < this.percent && this.percent < 90) {
        return "blue";
      } else if (91 < this.percent && this.percent - 100) {
        return "yellow";
      } else {
        return "";
      }
    },
    clear() {
      if (this.count == 0) {
        return "none";
      }
      return "block";
    },
  },
  methods: {
    addItem() {
      if (this.item.id) {
        for (let i = 0; i < this.todolist.length; i++) {
          if (this.item.id == this.todolist[i].id) {
            this.item.nameJob = this.job;
            this.todolist.splice(i, 1, this.item);
          }
        }
      } else {
        (this.item = {
          id: Math.floor(Math.random() * 1000),
          nameJob: this.job,
          action: false,
        }),
          this.todolist.push(this.item);
      }

      this.item = {
        id: "",
        nameJob: "",
      };
      this.job = "";
    },
    checkBox(item) {
      item.action = !item.action;
      this.totalCount();
    },
    totalCount() {
      this.count = 0;
      for (let i = 0; i < this.todolist.length; i++) {
        if (this.todolist[i].action == true) {
          this.count++;
        }
      }
    },

    deleteItem(id) {
      for (let i = 0; i < this.todolist.length; i++) {
        if (this.todolist[i].id == id) {
          if (this.todolist[i].action) {
            this.todolist.splice(i, 1);
            this.count--;
          } else {
            this.todolist.splice(i, 1);
          }
        }
      }
    },
    editItem(val) {
      this.item = val;
      this.job = this.item.nameJob;
    },

    removeAll() {
      for (let i = 0; i < this.todolist.length; i++) {
        if (this.todolist[i].action === true) {
          this.todolist.splice(i, 1);
          this.count--;
          i--;
        }
      }
    },
  },
  data() {
    return {
      percent: 0,
      todolist: [],
      item: {
        id: Math.floor(Math.random() * 1000),
        nameJob: "",
        action: false,
      },
      job: "",
      count: 0,
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/* RESET STYLES*/
html,
body,
div,
span,
object,
iframe,
h1,
h2,
h3,
h4,
h5,
h6,
p,
blockquote,
pre,
abbr,
address,
cite,
code,
del,
dfn,
em,
img,
ins,
kbd,
q,
samp,
small,
strong,
sub,
sup,
var,
b,
i,
dl,
dt,
dd,
ol,
ul,
li,
fieldset,
form,
label,
legend,
table,
caption,
tbody,
tfoot,
thead,
tr,
th,
td,
article,
aside,
canvas,
details,
figcaption,
figure,
footer,
header,
hgroup,
menu,
nav,
section,
summary,
time,
mark,
audio,
video {
  margin: 0;
  padding: 0;
  border: 0;
  outline: 0;
  font-size: 100%;
  vertical-align: baseline;
  background: transparent;
}
ol,
ul {
  list-style: none;
}

/* ================================ Layout ================================ */

body {
  background-color: #499cc1;
  text-align: left;
  color: #333;
  font: 13px Tahoma, sans-serif;
}
.wrapper {
  position: relative;
  width: 500px;
  margin: 0 auto;
  background: #fcfcfc;
}

/* ================================ EO Layout ================================ */

/* ================================ Header ================================ */

header {
  font: 25px/50px Tahoma, sans-serif;
  text-transform: uppercase;
  color: #555;
  text-align: center;
  text-shadow: 0 0 1px #499cc1;
}

/* ================================ EO Header ================================ */

/* ================================ Content ================================ */

.ToDo {
  padding: 0 30px 10px 30px;
}

.add {
  width: 383px;
  height: 38px;
  border: 1px solid #58a3c7;
  text-align: center;
  font: italic 18px Arial, sans-serif;
  color: #08638e;
}

.add-btn {
  height: 42px;
  border: 1px solid #58a3c7;
  background-color: #58a3c7;
  font: italic 18px/40px Arial, sans-serif;
  color: #fff;
  margin: 0;
  padding: 0 16px;
  cursor: pointer;
}

/*======LIST======*/

.task-list {
  margin: 10px 0 30px 0;
  color: #4a9ac0;
}

.task-list li {
  background: #f0f9ff;
  padding: 10px 10px 4px;
  border-bottom: 1px solid #e9e9e9;
}
.task-list .done {
  background: #f9fdff;
}

.task-list li:first-child {
  border-top: 1px solid #e9e9e9;
}

.task-list .toggle {
  margin: 0 10px 0 0;
  vertical-align: top;
}

.task-title {
  position: relative;
  top: -3px;
  display: inline-block;
  padding: 2px;
  border: 1px solid transparent;
  width: 352px;
  text-align: left;
  line-height: 13px;
}

.editing {
  background-color: #fff;
}
.done .editing {
  text-decoration: line-through;
  border-color: #f0f9ff;
}

.done .task-title {
  text-decoration: line-through;
}

.remove {
  float: right;
  width: 12px;
  height: 12px;
  text-indent: -9999px;
  margin: 0 0 0 10px;
}

.remove:hover {
}

.edit {
  float: right;
  width: 12px;
  height: 13px;
  text-indent: -9999px;
  margin: 0;
}

.edit:hover {
}
/* ======EO LIST====== */

.bottom {
  overflow: hidden;
}

.progress-bar {
  display: none;
  float: left;
  width: 200px;
  height: 26px;
  border: 1px solid #e9e9e9;
  overflow: hidden;
}

.progress-status {
  height: 100%;
  background: #5a5;
  -webkit-transition: all 0.5s ease-in;
  transition: all 0.5s ease-in;
}

.progress-status p {
  width: 200px;
  text-align: center;
  color: #666;
  line-height: 25px;
}

.progress-status p span {
  font-weight: bold;
}

.remove-checked {
  display: none;
  text-decoration: none;
  float: right;
  background: #4a9ac0;
  margin: 0 0 0 20px;
  padding: 6px 10px 6px 20px;
}

.remove-checked span {
  display: inline-block;
  font: 14px/14px Arial, sans-serif;
  color: #fff;
  padding: 0 20px 0 0;
}

/* ================================ EO Content ================================ */

/* ================================ Footer ================================ */

footer {
  display: none;
  margin: 30px 0 5px 0;
  color: #999;
  text-align: center;
  font-size: 11px;
}
footer a {
  color: #777;
}

/* ================================ EO Footer ================================ */

/* ================================ Scroll Bar ================================ */
::-webkit-scrollbar {
  width: 12px;
}

/* Track */
::-webkit-scrollbar-track {
  -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.5);
  -webkit-border-radius: 10px;
  border-radius: 10px;
}

/* Handle */
::-webkit-scrollbar-thumb {
  -webkit-border-radius: 10px;
  border-radius: 10px;
  background: rgba(0, 150, 230, 0.8);
  -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.7);
}
::-webkit-scrollbar-thumb:window-inactive {
  background: rgba(0, 150, 230, 0.4);
}
/* ================================ Scroll Bar ================================ */

h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
