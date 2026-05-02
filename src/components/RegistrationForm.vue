<script setup>
import { reactive, ref } from 'vue';

// สร้างตัวแปรเก็บข้อมูลฟอร์มด้วย reactive
const formData = reactive({
  caregiver: {
    name: '',
    relation: '',
    phone: '',
    lineId: ''
  },
  dependent: {
    name: '',
    age: '',
    condition: ''
  },
  pdpaConsent: false
});

// ฟังก์ชันเมื่อกดปุ่ม Submit
const submitForm = () => {
  if (!formData.pdpaConsent) {
    alert('กรุณายืนยันนโยบายความเป็นส่วนตัว');
    return;
  }
  
  // ในอนาคตคุณสามารถนำ formData ไปยิง API (Axios/Fetch) ส่งหา Node.js ได้ที่นี่
  console.log('ข้อมูลที่เตรียมส่งเข้าระบบ AFE Plus:', JSON.parse(JSON.stringify(formData)));
  alert('บันทึกข้อมูลเรียบร้อยแล้ว (ตรวจสอบข้อมูลใน Console)');
};
</script>

<template>
  <div class="organized-form-container">
    <div class="form-header">
      <h1>ลงทะเบียนระบบ AFE Plus</h1>
      <p>กรุณากรอกข้อมูลเพื่อใช้ในการดูแลและแจ้งเตือน</p>
    </div>

    <!-- ใช้ @submit.prevent แทน event.preventDefault() -->
    <form @submit.prevent="submitForm">
      
      <!-- ส่วนที่ 1: ข้อมูลผู้ดูแล -->
      <div class="form-section">
        <h3 class="section-title">ส่วนที่ 1: ข้อมูลผู้ดูแล (Caregiver)</h3>
        
        <div class="grid-row grid-2-col">
          <div class="input-group">
            <label>ชื่อ-นามสกุล <span class="req">*</span></label>
            <!-- ผูกข้อมูลด้วย v-model -->
            <input type="text" v-model="formData.caregiver.name" required placeholder="ชื่อผู้รับผิดชอบหลัก">
          </div>
          <div class="input-group">
            <label>ความสัมพันธ์ <span class="req">*</span></label>
            <input type="text" v-model="formData.caregiver.relation" required placeholder="เช่น บุตร, หลาน, ญาติ">
          </div>
        </div>

        <div class="grid-row grid-2-col">
          <div class="input-group">
            <label>เบอร์โทรศัพท์ <span class="req">*</span></label>
            <input type="tel" v-model="formData.caregiver.phone" required pattern="[0-9]{10}" placeholder="08XXXXXXXX">
          </div>
          <div class="input-group">
            <label>LINE ID</label>
            <input type="text" v-model="formData.caregiver.lineId" placeholder="สำหรับการแจ้งเตือน">
          </div>
        </div>
      </div>

      <!-- ส่วนที่ 2: ข้อมูลผู้มีภาวะพึ่งพิง -->
      <div class="form-section">
        <h3 class="section-title">ส่วนที่ 2: ข้อมูลผู้มีภาวะพึ่งพิง (Dependent)</h3>
        
        <div class="grid-row grid-2-col">
          <div class="input-group">
            <label>ชื่อ-นามสกุล <span class="req">*</span></label>
            <input type="text" v-model="formData.dependent.name" required placeholder="ชื่อผู้รับการดูแล">
          </div>
          <div class="input-group">
            <label>อายุ (ปี) <span class="req">*</span></label>
            <input type="number" v-model="formData.dependent.age" required min="1" max="120" placeholder="เช่น 75">
          </div>
        </div>

        <div class="grid-row grid-1-col">
          <div class="input-group">
            <label>โรคประจำตัว / ข้อควรระวัง</label>
            <textarea v-model="formData.dependent.condition" rows="2" placeholder="ระบุข้อมูลที่จำเป็นต่อการดูแล (ถ้ามี)"></textarea>
          </div>
        </div>
      </div>

      <!-- ส่วนที่ 3: PDPA -->
      <div class="form-section pdpa-section">
        <label class="checkbox-wrapper">
          <input type="checkbox" v-model="formData.pdpaConsent" required>
          <span class="checkbox-text">
            ข้าพเจ้ายินยอมให้ระบบเก็บบันทึกข้อมูลเพื่อใช้ในการแจ้งเตือนและดูแลตามนโยบายความเป็นส่วนตัว <span class="req">*</span>
          </span>
        </label>
      </div>

      <div class="form-actions">
        <button type="submit" class="btn-submit">บันทึกข้อมูล</button>
      </div>
    </form>
  </div>
</template>

<style scoped>
/* นำ CSS ทั้งหมดที่ผมเคยให้ไว้ในหัวข้อที่แล้ว (ที่เป็นสีเขียว/Grid) มาใส่ในนี้ได้เลยครับ */
.organized-form-container {
    max-width: 700px;
    margin: 0 auto;
    background: #FFFFFF;
    border-radius: 8px;
    box-shadow: 0 4px 12px rgba(0,0,0,0.08);
    padding: 30px;
}
.form-header { text-align: center; margin-bottom: 25px; border-bottom: 1px solid #E0E0E0; padding-bottom: 15px; }
.form-header h1 { color: #333; font-size: 1.5rem; margin: 0 0 5px 0; }
.form-header p { color: #666; font-size: 0.95rem; margin: 0; }
.form-section { margin-bottom: 25px; }
.section-title { background-color: #E8F5E9; color: #2E7D32; padding: 8px 12px; font-size: 1.05rem; margin: 0 0 15px 0; border-radius: 4px; border-left: 4px solid #00B900; }
.grid-row { display: grid; gap: 15px; margin-bottom: 15px; }
.grid-1-col { grid-template-columns: 1fr; }
.grid-2-col { grid-template-columns: 1fr 1fr; }
.input-group label { display: block; font-size: 0.9rem; font-weight: 600; margin-bottom: 5px; color: #333; text-align: left; }
.req { color: #D32F2F; }
input[type="text"], input[type="tel"], input[type="number"], textarea { width: 100%; padding: 10px; border: 1px solid #E0E0E0; border-radius: 4px; font-size: 0.95rem; background-color: #FAFAFA; box-sizing: border-box; }
input:focus, textarea:focus { outline: none; border-color: #00B900; background-color: #FFFFFF; box-shadow: 0 0 0 2px rgba(0, 185, 0, 0.15); }
.pdpa-section { background-color: #F9F9F9; padding: 15px; border-radius: 4px; border: 1px dashed #CCC; text-align: left; }
.checkbox-wrapper { display: flex; align-items: center; gap: 10px; cursor: pointer; }
.checkbox-wrapper input[type="checkbox"] { width: 18px; height: 18px; cursor: pointer; }
.checkbox-text { font-size: 0.9rem; color: #666; }
.form-actions { text-align: center; margin-top: 30px; }
.btn-submit { background-color: #00B900; color: #FFFFFF; border: none; padding: 12px 30px; font-size: 1.05rem; font-weight: bold; border-radius: 6px; cursor: pointer; width: 100%; max-width: 300px; transition: opacity 0.2s; }
.btn-submit:hover { opacity: 0.9; }
@media (max-width: 500px) { .grid-2-col { grid-template-columns: 1fr; } .organized-form-container { padding: 20px; } }
</style>