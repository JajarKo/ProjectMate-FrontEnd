<template>
  <div class="album py-5 bg-light">
    <div class="container">
      <div class="pb-3 mb-4 border-bottom">
        <a
          href="/"
          class="d-flex align-items-center text-dark text-decoration-none"
        >
          <i class="bi bi-gem me-2" style="font-size: 32px"></i>
          <span class="fs-4">ทำไมต้องเรา</span>
        </a>
      </div>

      <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 row-cols-md-4 g-3">
        <div class="col text-center">
          <img
            src="@/assets/reliability.png"
            class="why-img m-3"
            alt="reliability"
          />
          <h4 class="fw-normal">คุณภาพดี</h4>
          <p>เรามีระบบคัดกรองประวัติเบื้องต้นของผู้ใช้ และโปรเจคงาน</p>
        </div>

        <div class="col text-center">
          <img src="@/assets/winner.png" class="why-img m-3" alt="winner" />
          <h4 class="fw-normal">น่าเชื่อถือ</h4>
          <p>เราเป็นแหล่งรวบรวมงานและบุคลากรที่ผู้ใช้ไว้วางใจ</p>
        </div>

        <div class="col text-center">
          <img
            src="@/assets/easy-to-use.png"
            class="why-img m-3"
            alt="easy-to-use"
          />
          <h4 class="fw-normal">ใช้งานง่าย</h4>
          <p>
            สามารถค้นหาประเภทงานหรือเพื่อนร่วมงานคุณได้ง่ายๆรวมถึงสร้างโพสต์ของตัวเอง
          </p>
        </div>

        <div class="col text-center">
          <img
            src="@/assets/padlock.png"
            class="why-img m-3"
            alt="reliability"
          />
          <h4 class="fw-normal">ปลอดภัย</h4>
          <p>
            เรารับรองได้ว่างานและเพื่อนที่คุณตามหาไม่ใช่ผู้แอบอ้างหรือหลอกลวง
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
.why-img {
  width: 128px;
  height: 128px;
}
</style>
