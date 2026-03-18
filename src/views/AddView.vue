<script setup>
import axios from "axios";
import { reactive, ref } from "vue";

// ข้อมูลฟอร์ม
const data = reactive({
    fname: "",
    lname: "",
    email: "",
    phone: ""
});

// สถานะการโหลดและการแจ้งเตือน
const loading = ref(false);
const status = reactive({
    message: "",
    type: "" // 'success' หรือ 'danger'
});

const submitForm = async () => {
    loading.value = true;
    status.message = ""; // เคลียร์ข้อความเก่า

    try {
        const response = await axios.post("http://localhost:5678/webhook/data", data);
        console.log("Success:", response.data);

        // แจ้งเตือนเมื่อสำเร็จ
        status.message = "บันทึกข้อมูลสำเร็จเรียบร้อยแล้ว!";
        status.type = "success";

        // เคลียร์ค่าในฟอร์มหลังจากส่งสำเร็จ
        data.fname = "";
        data.lname = "";
        data.email = "";
        data.phone = "";

    } catch (error) {
        console.error("Error:", error);
        status.message = "เกิดข้อผิดพลาดในการส่งข้อมูล กรุณาลองใหม่อีกครั้ง";
        status.type = "danger";
    } finally {
        loading.value = false;
    }
};
</script>

<template>
    <div class="container mt-5 mb-5">
        <div class="row justify-content-center">
            <div class="col-12 col-md-8 col-lg-6">
                <div class="card border-0 shadow-lg rounded-4 overflow-hidden">

                    <div class="card-header bg-primary text-white text-center py-4 border-0"
                        style="background: linear-gradient(135deg, #0d6efd 0%, #0dcaf0 100%);">
                        <h4 class="mb-0 fw-bold">
                            <i class="bi bi-person-plus-fill me-2"></i>เพิ่มข้อมูลผู้ใช้งาน
                        </h4>
                        <p class="mb-0 mt-1 small opacity-75">กรอกรายละเอียดด้านล่างเพื่อส่งเข้า N8N Webhook</p>
                    </div>

                    <div class="card-body p-4 p-md-5">

                        <div v-if="status.message" :class="`alert alert-${status.type} alert-dismissible fade show`"
                            role="alert">
                            <i class="bi"
                                :class="status.type === 'success' ? 'bi-check-circle-fill' : 'bi-exclamation-triangle-fill'"></i>
                            <span class="ms-2">{{ status.message }}</span>
                            <button type="button" class="btn-close" @click="status.message = ''"></button>
                        </div>

                        <form @submit.prevent="submitForm" class="text-start">

                            <div class="row mb-3">
                                <div class="col-md-6 mb-3 mb-md-0">
                                    <label for="fname" class="form-label fw-semibold text-secondary small">ชื่อ <span
                                            class="text-danger">*</span></label>
                                    <div class="input-group">
                                        <span class="input-group-text bg-light border-end-0"><i
                                                class="bi bi-person text-muted"></i></span>
                                        <input type="text" id="fname" class="form-control border-start-0 ps-0"
                                            v-model="data.fname" placeholder="สมภพ" required>
                                    </div>
                                </div>
                                <div class="col-md-6">
                                    <label for="lname" class="form-label fw-semibold text-secondary small">นามสกุล <span
                                            class="text-danger">*</span></label>
                                    <input type="text" id="lname" class="form-control" v-model="data.lname"
                                        placeholder="เอี่ยมสมบัติ" required>
                                </div>
                            </div>

                            <div class="mb-3">
                                <label for="email" class="form-label fw-semibold text-secondary small">อีเมล <span
                                        class="text-danger">*</span></label>
                                <div class="input-group">
                                    <span class="input-group-text bg-light border-end-0"><i
                                            class="bi bi-envelope text-muted"></i></span>
                                    <input type="email" id="email" class="form-control border-start-0 ps-0"
                                        v-model="data.email" placeholder="sompop@riiixch.com" required>
                                </div>
                            </div>

                            <div class="mb-4">
                                <label for="phone"
                                    class="form-label fw-semibold text-secondary small">เบอร์โทรศัพท์</label>
                                <div class="input-group">
                                    <span class="input-group-text bg-light border-end-0"><i
                                            class="bi bi-telephone text-muted"></i></span>
                                    <input type="tel" id="phone" class="form-control border-start-0 ps-0"
                                        v-model="data.phone" placeholder="0956451925">
                                </div>
                            </div>

                            <div class="d-grid mt-2">
                                <button type="submit" class="btn btn-primary btn-lg rounded-pill fw-bold shadow-sm"
                                    :disabled="loading">
                                    <span v-if="loading" class="spinner-border spinner-border-sm me-2" role="status"
                                        aria-hidden="true"></span>
                                    <i v-else class="bi bi-send-fill me-2"></i>
                                    {{ loading ? 'กำลังส่งข้อมูล...' : 'บันทึกข้อมูล' }}
                                </button>
                            </div>

                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
/* ปรับแต่ง Input ให้ดูนุ่มนวลขึ้น */
.form-control:focus {
    border-color: #86b7fe;
    box-shadow: 0 0 0 0.25rem rgba(13, 110, 253, 0.15);
}

.input-group-text {
    border-color: #dee2e6;
}

.form-control {
    border-color: #dee2e6;
    padding: 0.6rem 0.75rem;
}
</style>