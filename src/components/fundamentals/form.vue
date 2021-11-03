<template>
  <div>
    <div class="row justify-content-center">
      <div class="col-4">
        <b-table
          light
          hover
          selectable
          select-mode="multi"
          :items="info"
          :fields="fields"
          @row-dblclicked="onRowDblClicked"
          @row-selected="onRowSelected"
        >
          <template #cell(id)="data">
            {{ getNumber(data) }}
          </template>
          <template #cell(gender)="data">
            {{ getGender(data).text }}
          </template>
        </b-table>
      </div>
    </div>
    <div class="row justify-content-center">
      <div class="col-5">
        <div v-if="message.show">
          {{ message.content }}
        </div>
      </div>
      <div class="col-3">
        <b-button
          v-if="deleteRange.show"
          variant="outline-danger"
          @click="onDeleteRange"
        >
          {{ deleteRange.content }}
        </b-button>
      </div>
    </div>
    <hr />
    <div class="row justify-content-center">
      <div class="col-md-3">
        <div class="row mb-3">
          <div class="col-4">Tên <span class="text-danger">*</span></div>
          <div class="col-8">
            <input
              v-model="form.name"
              id="name"
              type="text"
              class="form-control"
              placeholder="Tên"
              required
            />
          </div>
        </div>
        <div class="row mb-5">
          <div class="col-4">Giới tính <span class="text-danger">*</span></div>
          <div class="col-8">
            <select
              id="gender"
              name="gender"
              v-model="form.gender"
              class="form-control"
              required
            >
              <option
                v-for="gender in genderList"
                :key="gender.value"
                :value="gender.value"
              >
                {{ gender.text }}
              </option>
            </select>
          </div>
        </div>
        <div class="row justify-content-center">
          <div class="col">
            <b-button variant="outline-dark" @click="resetForm">Reset</b-button>
          </div>
          <div class="col">
            <b-button variant="outline-primary" @click="onSubmit">Lưu</b-button>
          </div>
          <div class="col">
            <b-button
              variant="outline-danger"
              @click="onDelete"
              :disabled="form.id === 0 ? true : false"
            >
              Xóa
            </b-button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "form",
  data() {
    return {
      selectedRows: [],
      deleteRange: {
        show: false,
        content: ""
      },
      fields: [
        {
          key: "id",
          label: "STT",
          sortable: false
        },
        {
          key: "name",
          label: "Tên",
          sortable: false
        },
        {
          key: "gender",
          label: "Giới tính",
          sortable: false
        }
      ],
      message: {
        show: false,
        content: ""
      },
      info: [
        { id: 1, name: "Huy", gender: 1 },
        { id: 2, name: "Thu", gender: 2 },
        { id: 3, name: "Đinh", gender: 3 }
      ],
      genderList: [
        { text: "Chọn giới tính", value: 0 },
        { text: "Nam", value: 1 },
        { text: "Nữ", value: 2 },
        { text: "Khác", value: 3 }
      ],
      form: {
        id: 0,
        name: "",
        gender: 0
      }
    };
  },
  methods: {
    getNumber(data) {
      return data.index + 1;
    },
    getGender(data) {
      return this.genderList.find(o =>
        o.value === data.item.gender ? o.text : null
      );
    },
    onDeleteRange() {
      if (confirm("Xóa (" + this.selectedRows.length + ") mục đã chọn ?")) {
        this.info = this.info.filter(o => !this.selectedRows.includes(o));
        setTimeout(() => {
          Object.assign(this.message, {
            show: true,
            content: "Xoá (" + this.selectedRows.length + ") thành công!"
          });
          this.resetForm();
          setTimeout(() => {
            Object.assign(this.message, {
              show: false,
              content: ""
            });
          }, 2000);
        }, 500);
      }
    },
    onRowSelected(rows) {
      if (rows.length > 0) {
        this.selectedRows = rows;
        Object.assign(this.deleteRange, {
          show: true,
          content: "Xóa ( " + this.selectedRows.length + " ) bản ghi"
        });
      } else {
        this.deleteRange.show = false;
      }
    },
    onRowDblClicked(item) {
      Object.assign(this.form, item);
    },
    onDelete() {
      if (confirm("Xóa " + this.form.name + " ?")) {
        this.info.forEach(o =>
          o.id === this.form.id
            ? this.info.splice(this.info.indexOf(o.id), 1)
            : null
        );
        setTimeout(() => {
          Object.assign(this.message, {
            show: true,
            content: "Xoá thành công!"
          });
          this.resetForm();
          setTimeout(() => {
            Object.assign(this.message, {
              show: false,
              content: ""
            });
          }, 2000);
        }, 500);
      }
    },
    onSubmit() {
      if (this.form.name && this.form.gender) {
        if (this.form.id === 0) {
          this.form.id = this.info.length + 1;
          let obj = {};
          Object.assign(obj, this.form);
          this.info.push(obj);
          this.resetForm();
          setTimeout(() => {
            Object.assign(this.message, {
              show: true,
              content: "Thêm mới '" + obj.name + "' thành công!"
            });
            setTimeout(() => {
              Object.assign(this.message, {
                show: false,
                content: ""
              });
            }, 2000);
          }, 500);
        } else {
          this.info.forEach(o =>
            o.id === this.form.id ? Object.assign(o, this.form) : null
          );
          setTimeout(() => {
            Object.assign(this.message, {
              show: true,
              content: "Cập nhật '" + this.form.name + "' thành công!"
            });
            this.resetForm();
            setTimeout(() => {
              Object.assign(this.message, {
                show: false,
                content: ""
              });
            }, 2000);
          }, 500);
        }
      } else {
        setTimeout(() => {
          Object.assign(this.message, {
            show: true,
            content: "Không thành công, kiểm tra lại thông tin"
          });
          setTimeout(() => {
            Object.assign(this.message, {
              show: false,
              content: ""
            });
          }, 4000);
        }, 500);
      }
    },
    resetForm() {
      Object.assign(this.form, {
        id: 0,
        name: "",
        gender: 0
      });
    }
  }
};
</script>

<style>
</style>