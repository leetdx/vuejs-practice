<template>
  <div>
    <section class="wrapper">
      <header><h1>ToDoList</h1></header>
      <!-- content -->
      <div class="ToDo">
        <form action="#" spellcheck="false">
          <!-- <label for="task-input">Type</label> -->
          <input
            v-model="input.text"
            type="text"
            id="task-input"
            placeholder="what needs to be done?"
            class="add"
            @keyup.enter="onSubmit"
          />
          <input
            type="submit"
            id="task-submit"
            value="+"
            class="add-btn"
            @click="onSubmit"
          />
        </form>
        <ul class="task-list">
          <li
            class=""
            v-for="todo in todoList"
            :key="todo.id"
            :style="`background: ${todo.done ? '#f9fdff' : '#f0f9ff'};`"
          >
            <input
              v-model="todo.done"
              type="checkbox"
              class="toggle"
              @click="onSelect(todo)"
            />
            <span
              id="task-2"
              :contenteditable="editable"
              :style="`text-decoration: ${todo.done ? 'line-through' : 'none'}`"
              spellcheck="false"
              class="task-title"
              v-on:click.self="editable = true"
              >{{ todo.text }}
            </span>
            <a href="#" class="remove" @click="onDelete(todo)">delete</a>
            <a href="#" class="edit" @click="onEdit(todo)">edit</a>
          </li>
        </ul>
        <div class="bottom">
          <div v-show="todoList.length" class="progress-bar">
            <div class="progress-status" :style="computedProcess">
              <p>
                <span class="tasks-left">{{ doneCount }}</span> of
                <span class="tasks-total">{{ todoList.length }}</span> tasks
                done
              </p>
            </div>
          </div>
          <a
            v-show="doneCount > 0"
            href="#"
            class="remove-checked"
            @click="onDeleteChecked"
            ><span>Remove checked</span></a
          >
        </div>
      </div>
      <!-- EO content -->
      <footer>
        <p>
          Created by: <a href="https://github.com/Edifear/ToDoList/">Edifear</a>
        </p>
        <p>Designed by: <span>Serafim</span></p>
      </footer>
    </section>
  </div>
</template>

<script>
export default {
  data() {
    return {
      progessBar: {
        display: "inline-block"
      },
      editable: false,
      input: { id: 0, text: "", done: false },
      todoList: []
    };
  },
  computed: {
    computedProcess() {
      const percentage = (this.doneCount * 100) / this.todoList.length;
      let r = 0;
      let g = 0;
      if (percentage < 50) {
        r = 255;
        g = Math.round(5.1 * percentage);
      } else {
        g = 255;
        r = Math.round(510 - 5.1 * percentage);
      }
      return {
        width: `${percentage > 0 ? percentage : 0}%`,
        height: 100,
        color: `rgba(${r}, ${g}, 0, 0.7)`
      };
    },
    doneCount() {
      return this.todoList.filter(o => (o.done ? o : null)).length;
    }
  },
  methods: {
    onDeleteChecked() {
      if (confirm("Xóa mục đã chọn ?")) {
        this.todoList = this.todoList.filter(o => (!o.done ? o : null));
      }
    },
    onSelect(todo) {
      todo.done = !todo.done;
    },
    onEdit(todo) {
      this.input = todo;
    },
    onDelete(todo) {
      if (confirm("Xóa '" + todo.text + "' ? ")) {
        this.todoList = this.todoList.filter(o => o !== todo);
      }
    },
    onSubmit() {
      if (this.input.id === 0) {
        let temp = new Date();
        this.input.id = temp.getTime();
        const obj = Object.assign({}, this.input);
        this.todoList.push(obj);
      } else {
        this.todoList.forEach(o =>
          o.id == this.input.id ? Object.assign(o, this.input) : null
        );
      }
      this.resetInput();
    },
    resetInput() {
      this.input.id = 0;
      this.input.text = "";
    }
  }
};
</script>

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
  background: url(../../assets/images/sprite.png) no-repeat 0 -11px;
  margin: 0 0 0 10px;
}

.remove:hover {
  background: url(../../assets/images/sprite.png) no-repeat 0 0;
}

.edit {
  float: right;
  width: 12px;
  height: 13px;
  text-indent: -9999px;
  background: url(../../assets/images/sprite.png) no-repeat 0 -34px;
  margin: 0;
}

.edit:hover {
  background: url(../../assets/images/sprite.png) no-repeat 0 -22px;
}
/* ======EO LIST====== */

.bottom {
  overflow: hidden;
}

.progress-bar {
  display: inline-block;
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
  display: inline-block;
  text-decoration: none;
  float: right;
  background: #4a9ac0;
  margin: 0 0 0 20px;
  padding: 6px 10px 6px 20px;
}

.remove-checked span {
  display: inline-block;
  background: #4a9ac0 url("../../assets/images/sprite.png") no-repeat right -47px;
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
</style>