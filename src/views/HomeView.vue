<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

const users = ref([]);
const loading = ref(false);

const fetchData = async () => {
    loading.value = true;
    try {
        const response = await axios.get('http://localhost:5678/webhook/data');
        users.value = response.data;
    } catch (error) {
        console.error('Error fetching data:', error);
        // แจ้งเตือนแบบบ้านๆ ไปก่อน ถ้ามี SweetAlert2 ค่อยมาเปลี่ยนได้ครับ
        alert('ไม่สามารถดึงข้อมูลได้ กรุณาเช็คการตั้งค่า CORS ใน N8N Webhook');
    } finally {
        // หน่วงเวลาเล็กน้อยให้เห็น Loading สวยๆ (ถ้า Data มาไวเกิน)
        setTimeout(() => {
            loading.value = false;
        }, 500);
    }
};

onMounted(() => {
    fetchData();
});
</script>

<template>
    <div class="container mt-5">
        <div class="card border-0 shadow-lg rounded-4 overflow-hidden">

            <div
                class="card-header custom-header text-white d-flex justify-content-between align-items-center py-3 px-4">
                <h5 class="mb-0 fw-bold d-flex align-items-center gap-2">
                    <i class="bi bi-person-lines-fill fs-4"></i>
                    รายชื่อผู้ใช้งาน
                    <span class="badge bg-white text-primary rounded-pill ms-2 fs-6 opacity-75">Webhook</span>
                </h5>
                <button @click="fetchData"
                    class="btn btn-light btn-sm fw-semibold rounded-pill px-3 shadow-sm d-flex align-items-center gap-2"
                    :disabled="loading">
                    <span v-if="loading" class="spinner-border spinner-border-sm"></span>
                    <i v-else class="bi bi-arrow-clockwise"></i>
                    {{ loading ? 'กำลังอัปเดต...' : 'รีเฟรชข้อมูล' }}
                </button>
            </div>

            <div class="card-body p-0">
                <div class="table-responsive">
                    <table class="table table-hover align-middle mb-0 custom-table">
                        <thead class="table-light text-muted">
                            <tr>
                                <th scope="col" class="ps-4 py-3">ชื่อ-นามสกุล</th>
                                <th scope="col" class="py-3">อีเมล</th>
                                <th scope="col" class="py-3">เบอร์โทรศัพท์</th>
                                <th scope="col" class="pe-4 py-3 text-end">จัดการ</th>
                            </tr>
                        </thead>
                        <tbody>

                            <tr v-if="loading">
                                <td colspan="4" class="text-center py-5 text-muted">
                                    <div class="spinner-grow text-primary mb-3" role="status"></div>
                                    <div class="fw-semibold">กำลังเชื่อมต่อข้อมูลจาก N8N...</div>
                                </td>
                            </tr>

                            <tr v-else-if="users.length === 0">
                                <td colspan="4" class="text-center py-5 text-muted">
                                    <i class="bi bi-inbox text-black-50" style="font-size: 3rem;"></i>
                                    <h6 class="mt-3 fw-bold">ยังไม่มีข้อมูลผู้ใช้งาน</h6>
                                    <p class="small mb-0">กดปุ่มรีเฟรชด้านบนเพื่อดึงข้อมูลล่าสุด</p>
                                </td>
                            </tr>

                            <tr v-for="(user, index) in users" :key="index" class="table-row-hover">
                                <td class="ps-4">
                                    <div class="d-flex align-items-center gap-3">
                                        <div
                                            class="avatar-circle bg-primary-subtle text-primary fw-bold d-flex align-items-center justify-content-center">
                                            {{ user.fname.charAt(0) }}
                                        </div>
                                        <div>
                                            <div class="fw-bold text-dark">{{ user.fname }} {{ user.lname }}</div>
                                        </div>
                                    </div>
                                </td>
                                <td>
                                    <a :href="'mailto:' + user.email"
                                        class="text-decoration-none text-secondary d-flex align-items-center gap-2 email-link">
                                        <i class="bi bi-envelope"></i>
                                        {{ user.email }}
                                    </a>
                                </td>
                                <td>
                                    <span
                                        class="badge bg-info-subtle text-info-emphasis rounded-pill px-3 py-2 fw-semibold border border-info-subtle">
                                        <i class="bi bi-telephone-fill me-1"></i>
                                        {{ user.telephone }}
                                    </span>
                                </td>
                                <td class="pe-4 text-end">
                                    <button class="btn btn-outline-secondary btn-sm rounded-circle me-1"
                                        title="ดูรายละเอียด">
                                        <i class="bi bi-eye"></i>
                                    </button>
                                </td>
                            </tr>

                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
/* แต่ง Header ให้ไล่สีดู Modern */
.custom-header {
    background: linear-gradient(135deg, #0d6efd 0%, #0dcaf0 100%);
    border-bottom: none;
}

/* แต่งหัวตาราง */
.custom-table th {
    font-weight: 600;
    text-transform: uppercase;
    font-size: 0.8rem;
    letter-spacing: 0.8px;
    background-color: #f8f9fa;
    border-bottom: 2px solid #e9ecef;
}

/* สร้างวงกลม Avatar */
.avatar-circle {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    font-size: 1.1rem;
}

/* Hover Effect สำหรับแถวในตาราง */
.table-row-hover {
    transition: all 0.2s ease;
}

.table-row-hover:hover {
    background-color: #f8f9fa !important;
    transform: translateY(-1px);
}

/* Hover Effect สำหรับ Email */
.email-link {
    transition: color 0.2s ease;
}

.email-link:hover {
    color: #0d6efd !important;
}

/* สำหรับ Bootstrap 5.3 Utility classes แบบ Fallback ถ้าระบบยังเป็นเวอร์ชันเก่า */
.bg-primary-subtle {
    background-color: #cfe2ff !important;
}

.bg-info-subtle {
    background-color: #cff4fc !important;
}

.text-info-emphasis {
    color: #055160 !important;
}
</style>