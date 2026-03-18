<template>
    <div id="app-container">
        <nav class="navbar navbar-expand-lg navbar-light bg-white sticky-top py-2 border-bottom shadow-sm">
            <div class="container">

                <router-link class="navbar-brand fw-bold d-flex align-items-center gap-2 text-primary" to="/">
                    <div
                        class="bg-primary text-white rounded-3 d-flex align-items-center justify-content-center logo-icon shadow-sm">
                        <i class="bi bi-layers-fill"></i>
                    </div>
                    <span>Webhook<span class="text-dark">App</span></span>
                </router-link>

                <button class="navbar-toggler border-0 shadow-none" type="button" data-bs-toggle="collapse"
                    data-bs-target="#navbarNav">
                    <span class="navbar-toggler-icon"></span>
                </button>

                <div class="collapse navbar-collapse" id="navbarNav">
                    <ul class="navbar-nav ms-auto gap-1 mt-3 mt-lg-0">
                        <li class="nav-item">
                            <router-link to="/" class="nav-link px-3 fw-medium transition-all"
                                exact-active-class="active-menu">
                                <i class="bi bi-table me-1"></i> ตารางผู้ใช้งาน
                            </router-link>
                        </li>
                        <li class="nav-item">
                            <router-link to="/add" class="nav-link px-3 fw-medium transition-all"
                                exact-active-class="active-menu">
                                <i class="bi bi-person-plus me-1"></i> เพิ่มข้อมูล
                            </router-link>
                        </li>
                    </ul>
                </div>

            </div>
        </nav>

        <main class="main-content min-vh-100 bg-light pb-5 pt-3">
            <router-view />
        </main>
    </div>
</template>

<script setup>
// ไม่จำเป็นต้องใส่ CSS ของ Bootstrap ที่นี่ เพราะปกติจะใส่ใน main.js
// แต่ถ้ายังไม่ได้ติด ให้ใส่ <link href="..."> ใน index.html
</script>

<style>
/* Reset บางอย่างของ Vue ที่ติดมากับโปรเจกต์เริ่มต้น */
#app {
    font-family: 'Sarabun', 'Prompt', Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    /* ลบ text-align: center ออก เพื่อไม่ให้ Layout Bootstrap เพี้ยน */
}

/* ตั้งค่าพื้นหลังของทั้งหน้าให้เป็นสีเทาอ่อน จะได้ขับให้ Card สีขาวดูโดดเด่น */
body {
    background-color: #f8f9fa;
    margin: 0;
}

/* เอฟเฟกต์สำหรับเมนู */
.transition-all {
    transition: all 0.2s ease;
}

/* ปรับแต่งความกว้างของโลโก้ */
.logo-icon {
    width: 32px;
    height: 32px;
    font-size: 1.1rem;
}

/* สไตล์ของเมนูทั่วไปตอนนำเมาส์ไปชี้ */
.nav-link {
    color: #555 !important;
    position: relative;
}

/* สไตล์ของเมนูที่กำลังเปิดอยู่ (Active State) แบบเส้นขีดด้านล่าง */
.active-menu {
    color: #0d6efd !important;
    font-weight: 600 !important;
}

/* สร้างเส้นขีดด้านล่างเมนูที่ Active */
.nav-item .active-menu::after {
    content: '';
    position: absolute;
    width: 100%;
    height: 2px;
    bottom: -2px;
    left: 0;
    background-color: #0d6efd;
    visibility: visible;
    border-radius: 2px;
}
</style>