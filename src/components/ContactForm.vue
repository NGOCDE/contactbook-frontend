<template>
    <Form @submit="submitContact" :validation-schema="contactFormSchema">
        <div class="form-group">
            <label for="name"><i class="fa-solid fa-user pr-1"></i>Tên</label>
            <Field name="name" type="text" class="form-control" v-model="contactLocal.name" />
            <ErrorMessage name="name" class="error-feedback" />
        </div>
        <div class="form-group">
            <label for="email"><i class="fa-solid fa-envelope pr-1"></i>E-mail</label>
            <Field name="email" type="email" class="form-control" v-model="contactLocal.email" />
            <ErrorMessage name="email" class="error-feedback" />
        </div>
        <div class="form-group">
            <label for="address"><i class="fa-solid fa-location-dot pr-1"></i>Địa chỉ</label>
            <Field name="address" type="text" class="form-control" v-model="contactLocal.address" />
            <ErrorMessage name="address" class="error-feedback" />
        </div>
        <div class="form-group">
            <label for="phone"><i class="fa-solid fa-phone pr-1"></i>Điện thoại</label>
            <Field name="phone" type="tel" class="form-control" v-model="contactLocal.phone" />
            <ErrorMessage name="phone" class="error-feedback" />
        </div>
        <div class="form-group form-check">
            <input name="favorite" type="checkbox" class="form-check-input" v-model="contactLocal.favorite" />
            <label for="favorite" class="form-check-label">
                <strong>Liên hệ yêu thích</strong>
            </label>
        </div>
        <div class="form-group">
            <button class="btn btn-primary"><i class="fa-solid fa-floppy-disk pr-1"></i>Lưu</button>
            <button v-if="contactLocal._id" type="button" class="ml-2 btn btn-danger" @click="deleteContact">
                <i class="fa-solid fa-trash pr-1"></i>Xóa
            </button>
        </div>
    </Form>

    
</template>
<script>
import * as yup from "yup";
import { Form, Field, ErrorMessage } from "vee-validate";
export default {
    components: {
        Form,
        Field,
        ErrorMessage,
    },
    emits: ["submit:contact", "delete:contact"],
    props: {
        contact: { type: Object, required: true }
    },
    data() {
        const contactFormSchema = yup.object().shape({
            name: yup
                .string()
                .required("Tên phải có giá trị.")
                .min(2, "Tên phải ít nhất 2 ký tự.")
                .max(50, "Tên có nhiều nhất 50 ký tự."),
            email: yup
                .string()
                .email("E-mail không đúng.")
                .max(50, "E-mail tối đa 50 ký tự."),
            address: yup.string().max(100, "Địa chỉ tối đa 100 ký tự."),
            phone: yup
                .string()
                .matches(
                    /((09|03|07|08|05)+([0-9]{8})\b)/g,
                    "Số điện thoại không hợp lệ."
                ),
        });
        // Kiểm tra xem có contact đã được truyền vào từ props hay chưa
        // Nếu có, sử dụng dữ liệu từ contact để chỉnh sửa, nếu không, tạo một liên hệ mới
        const contactLocal = this.contact ? { ...this.contact } : {
            name: "", // Giá trị mặc định của Tên
            email: "", // Giá trị mặc định của E-mail
            address: "", // Giá trị mặc định của Địa chỉ
            phone: "", // Giá trị mặc định của Điện thoại
            favorite: false, // Giá trị mặc định của Liên hệ yêu thích
            _id: null, // Giá trị mặc định của _id (null khi thêm mới)
        };

        return {
            contactLocal,
            contactFormSchema,
        };
        // return {
        //     // Chúng ta sẽ không muốn hiệu chỉnh props, nên tạo biến cục bộ
        //     // contactLocal để liên kết với các input trên form
        //     //contactLocal: this.contact,
        //     //contactFormSchema,
            
            
        // };
    },
    methods: {
        submitContact() {
            this.$emit("submit:contact", this.contactLocal);
        },
        deleteContact() {
            this.$emit("delete:contact", this.contactLocal.id);
        },
    },
};
</script>
<style scoped>
@import "@/assets/form.css";
</style>