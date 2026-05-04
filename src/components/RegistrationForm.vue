<script setup>
import { reactive } from 'vue';

const formData = reactive({
  caregiver: {
    firstName: '',
    lastName: '',
    password: '',
    confirmPassword: '',
    pin: '',
    address: '',
    phone: ''
  },
  dependent: {
    firstName: '',
    lastName: '',
    dob: '',
    gender: '',
    maritalStatus: '',
    address: '',
    phone: '',
    condition: '',
    medication: ''
  },
  pdpaConsent: false
});

const submitForm = () => {
  if (!formData.pdpaConsent) {
    alert('กรุณายืนยันนโยบายความเป็นส่วนตัว');
    return;
  }
  
  if (formData.caregiver.password !== formData.caregiver.confirmPassword) {
    alert('รหัสผ่านและการยืนยันรหัสผ่านไม่ตรงกัน กรุณาตรวจสอบอีกครั้ง');
    return;
  }

  const pinRegex = /^[0-9]{4}$/;
  if (!formData.caregiver.pin || !pinRegex.test(formData.caregiver.pin)) {
    alert('รหัส PIN ต้องเป็นตัวเลข 4 หลักเท่านั้น');
    return;
  }
  
  console.log('ข้อมูลที่เตรียมส่งเข้าระบบ AFE Plus V3:', JSON.parse(JSON.stringify(formData)));
  alert('บันทึกข้อมูลเรียบร้อยแล้ว');
};
</script>

<template>
  <div class="research-form-container">
    <div class="form-header">
      <div class="badge">AFE PLUS V.3</div>
      <h1>ลงทะเบียนดูแลผู้มีภาวะพึ่งพิง</h1>
      <p>กรุณากรอกข้อมูลให้ครบถ้วน เพื่อความแม่นยำในการแจ้งเตือนฉุกเฉิน</p>
    </div>

    <form @submit.prevent="submitForm">
      
      <!-- การ์ดที่ 1: ข้อมูลผู้ดูแล -->
      <div class="form-card">
        <div class="card-header">
          <span class="step-number">1</span>
          <h2>ข้อมูลผู้ดูแลหลัก (Caregiver)</h2>
        </div>
        
        <div class="grid-row grid-2-col">
          <div class="input-group">
            <label>ชื่อ <span class="req">*</span></label>
            <input type="text" v-model="formData.caregiver.firstName" required placeholder="ชื่อจริง">
          </div>
          <div class="input-group">
            <label>นามสกุล <span class="req">*</span></label>
            <input type="text" v-model="formData.caregiver.lastName" required placeholder="นามสกุล">
          </div>
        </div>

        <div class="grid-row grid-2-col">
          <div class="input-group">
            <label>ตั้งรหัสผ่าน <span class="req">*</span></label>
            <input type="password" v-model="formData.caregiver.password" required minlength="6" placeholder="อย่างน้อย 6 ตัวอักษร">
          </div>
          <div class="input-group">
            <label>ยืนยันรหัสผ่าน <span class="req">*</span></label>
            <input type="password" v-model="formData.caregiver.confirmPassword" required placeholder="กรอกรหัสผ่านอีกครั้ง">
          </div>
        </div>

        <div class="grid-row grid-2-col">
          <div class="input-group">
            <label>รหัส PIN สำหรับสมาร์ทวอทช์ <span class="req">*</span></label>
            <input type="password" v-model="formData.caregiver.pin" required inputmode="numeric" pattern="[0-9]{4}" maxlength="4" placeholder="ตัวเลข 4 หลัก">
          </div>
          <div class="input-group">
            <label>เบอร์โทรศัพท์มือถือ <span class="req">*</span></label>
            <input type="tel" v-model="formData.caregiver.phone" required inputmode="numeric" pattern="[0-9]{10}" placeholder="08XXXXXXXX">
          </div>
        </div>

        <div class="grid-row grid-1-col">
          <div class="input-group">
            <label>ที่อยู่ปัจจุบัน <span class="req">*</span></label>
            <textarea v-model="formData.caregiver.address" required rows="2" placeholder="บ้านเลขที่, ถนน, ตำบล, อำเภอ, จังหวัด, รหัสไปรษณีย์"></textarea>
          </div>
        </div>
      </div>

      <!-- การ์ดที่ 2: ข้อมูลผู้มีภาวะพึ่งพิง -->
      <div class="form-card">
        <div class="card-header">
          <span class="step-number">2</span>
          <h2>ข้อมูลผู้มีภาวะพึ่งพิง (Dependent)</h2>
        </div>
        
        <div class="grid-row grid-2-col">
          <div class="input-group">
            <label>ชื่อ <span class="req">*</span></label>
            <input type="text" v-model="formData.dependent.firstName" required placeholder="ชื่อจริง">
          </div>
          <div class="input-group">
            <label>นามสกุล <span class="req">*</span></label>
            <input type="text" v-model="formData.dependent.lastName" required placeholder="นามสกุล">
          </div>
        </div>

        <div class="grid-row grid-2-col">
          <div class="input-group">
            <label>วัน/เดือน/ปีเกิด <span class="req">*</span></label>
            <!-- UI ปฏิทินใหญ่ขึ้นตาม Fitts's Law -->
            <input type="date" v-model="formData.dependent.dob" required> 
          </div>
          <div class="input-group">
            <label>เพศ <span class="req">*</span></label>
            <select v-model="formData.dependent.gender" required>
              <option value="" disabled selected>กรุณาเลือกเพศ</option>
              <option value="ชาย">ชาย</option>
              <option value="หญิง">หญิง</option>
              <option value="อื่นๆ">อื่นๆ</option>
            </select>
          </div>
        </div>

        <div class="grid-row grid-2-col">
          <div class="input-group">
            <label>สถานภาพการสมรส</label>
            <select v-model="formData.dependent.maritalStatus">
              <option value="" disabled selected>กรุณาเลือกสถานภาพ</option>
              <option value="โสด">โสด</option>
              <option value="สมรส">สมรส</option>
              <option value="หย่าร้าง">หย่าร้าง</option>
              <option value="หม้าย">หม้าย</option>
            </select>
          </div>
          <div class="input-group">
            <label>เบอร์โทรศัพท์ (ถ้ามี)</label>
            <input type="tel" v-model="formData.dependent.phone" inputmode="numeric" pattern="[0-9]{10}" placeholder="08XXXXXXXX">
          </div>
        </div>

        <div class="grid-row grid-1-col">
          <div class="input-group">
            <label>ที่อยู่ปัจจุบัน (ใช้ในการอ้างอิงพิกัด) <span class="req">*</span></label>
            <textarea v-model="formData.dependent.address" required rows="2" placeholder="บ้านเลขที่, ถนน, ตำบล, อำเภอ, จังหวัด, รหัสไปรษณีย์"></textarea>
          </div>
        </div>

        <div class="grid-row grid-2-col">
          <div class="input-group">
            <label>โรคประจำตัว</label>
            <textarea v-model="formData.dependent.condition" rows="3" placeholder="ตัวอย่าง: ความดันโลหิตสูง, เบาหวาน"></textarea>
          </div>
          <div class="input-group">
            <label>ยาที่ใช้ประจำ</label>
            <textarea v-model="formData.dependent.medication" rows="3" placeholder="ตัวอย่าง: ยาลดความดัน (ทานหลังอาหารเช้า)"></textarea>
          </div>
        </div>
      </div>

      <!-- การ์ดที่ 3: ความยินยอม -->
      <div class="form-card pdpa-card">
        <label class="checkbox-wrapper">
          <input type="checkbox" v-model="formData.pdpaConsent" required>
          <div class="checkbox-content">
            <strong>นโยบายความเป็นส่วนตัว (PDPA)</strong>
            <p>ข้าพเจ้ายินยอมให้ระบบ AFE Plus เก็บรวบรวมและประมวลผลข้อมูลส่วนบุคคลและข้อมูลสุขภาพ เพื่อใช้ร่วมกับอุปกรณ์ Smartwatch ในการแจ้งเตือนฉุกเฉินตามวัตถุประสงค์ของระบบเท่านั้น <span class="req">*</span></p>
          </div>
        </label>
      </div>

      <div class="form-actions">
        <button type="submit" class="btn-submit">ยืนยันการลงทะเบียน</button>
      </div>
    </form>
  </div>
</template>

<style scoped>
/* =========================================
   UX/UI Research Implementation (WCAG 2.2, Fitts's Law)
========================================= */

.research-form-container {
    max-width: 800px;
    margin: 0 auto;
    padding: 20px 15px 60px;
    font-family: 'Sarabun', sans-serif;
    color: #222222; /* สีเข้ม High Contrast (WCAG) */
}

.form-header { 
    text-align: center; 
    margin-bottom: 30px; 
}

.badge {
    display: inline-block;
    background-color: #E8F5E9;
    color: #2E7D32;
    padding: 6px 16px;
    border-radius: 20px;
    font-size: 0.9rem;
    font-weight: bold;
    margin-bottom: 15px;
    letter-spacing: 1px;
}

.form-header h1 { 
    font-size: 1.8rem; 
    margin: 0 0 10px 0; 
    font-weight: 700;
}

.form-header p { 
    font-size: 1.1rem; 
    color: #555555; 
    margin: 0; 
}

/* Hick's Law: แบ่งข้อมูลเป็นการ์ดที่ดูแยกกันชัดเจน */
.form-card { 
    background: #FFFFFF; 
    border-radius: 12px; 
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.05); 
    padding: 30px; 
    margin-bottom: 25px; 
    border: 1px solid #EEEEEE;
}

.card-header {
    display: flex;
    align-items: center;
    gap: 15px;
    margin-bottom: 25px;
    padding-bottom: 15px;
    border-bottom: 2px solid #F0F0F0;
}

.step-number {
    background-color: #00B900;
    color: white;
    width: 36px;
    height: 36px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
    font-size: 1.2rem;
    font-weight: bold;
}

.card-header h2 {
    font-size: 1.3rem;
    margin: 0;
    color: #111111;
}

.grid-row { 
    display: grid; 
    gap: 20px; 
    margin-bottom: 20px; 
}
.grid-1-col { grid-template-columns: 1fr; }
.grid-2-col { grid-template-columns: 1fr 1fr; }

.input-group label { 
    display: block; 
    font-size: 1.1rem; /* ขนาดตัวอักษรใหญ่อ่านง่าย */
    font-weight: 600; 
    margin-bottom: 8px; 
}

.req { color: #E53935; font-weight: bold; }

/* Fitts's Law: พื้นที่สัมผัสขนาดใหญ่ (min-height: 48px) */
input[type="text"], 
input[type="tel"], 
input[type="password"], 
input[type="date"], 
select, 
textarea { 
    width: 100%; 
    min-height: 48px; /* พื้นที่กรอกขยายใหญ่ขึ้น */
    padding: 12px 15px; 
    border: 2px solid #DDDDDD; /* กรอบหนาเห็นชัดเจน */
    border-radius: 8px; 
    font-size: 1.1rem; 
    background-color: #FAFAFA; 
    box-sizing: border-box; 
    transition: all 0.3s ease;
    color: #222222;
}

/* WCAG: Focus State สว่างและชัดเจน */
input:focus, select:focus, textarea:focus { 
    outline: none; 
    border-color: #00B900; 
    background-color: #FFFFFF; 
    box-shadow: 0 0 0 4px rgba(0, 185, 0, 0.15); 
}

select {
    cursor: pointer;
    appearance: auto; 
}

/* PDPA Section */
.pdpa-card {
    background-color: #F4FBFC;
    border: 1px solid #BCE3EB;
}

.checkbox-wrapper { 
    display: flex; 
    align-items: flex-start; 
    gap: 15px; 
    cursor: pointer; 
}

.checkbox-wrapper input[type="checkbox"] { 
    width: 24px; /* กล่อง Checkbox ใหญ่ขึ้น */
    height: 24px; 
    margin-top: 2px; 
    cursor: pointer; 
    accent-color: #00B900;
}

.checkbox-content strong {
    display: block;
    font-size: 1.1rem;
    margin-bottom: 5px;
    color: #111;
}

.checkbox-content p { 
    font-size: 1rem; 
    color: #555; 
    line-height: 1.5; 
    margin: 0;
}

/* Fitts's Law: ปุ่ม Submit ขนาดใหญ่มาก กดง่าย */
.form-actions { 
    text-align: center; 
    margin-top: 40px; 
}

.btn-submit { 
    background-color: #00B900; 
    color: #FFFFFF; 
    border: none; 
    min-height: 56px; 
    padding: 0 40px; 
    font-size: 1.25rem; 
    font-weight: bold; 
    border-radius: 28px; /* ขอบมนเป็นมิตร */
    cursor: pointer; 
    width: 100%; 
    max-width: 400px; 
    box-shadow: 0 6px 12px rgba(0, 185, 0, 0.2);
    transition: transform 0.1s, background-color 0.2s; 
}

.btn-submit:active { transform: scale(0.98); }
.btn-submit:hover { background-color: #009900; }

/* Responsive สำหรับหน้าจอมือถือ (LINE App) */
@media (max-width: 600px) { 
    .grid-2-col { grid-template-columns: 1fr; gap: 15px; } 
    .form-card { padding: 20px; border-radius: 8px; }
    .research-form-container { padding: 15px 10px; }
}
</style>