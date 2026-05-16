<script setup>
import { reactive, watch } from 'vue';

const formData = reactive({
  caregiver: {
    firstName: '',
    lastName: '',
    password: '',
    confirmPassword: '',
    pin: '',
    phone: '',
    // แบ่งฟิลด์ที่อยู่เป็นสัดส่วน
    address: {
      houseNo: '',
      moo: '',
      soi: '',
      road: '',
      subdistrict: '', // ตำบล
      district: '',    // อำเภอ
      province: '',    // จังหวัด
      zipcode: ''
    }
  },
  dependent: {
    firstName: '',
    lastName: '',
    dob: '',
    gender: '',
    maritalStatus: '',
    phone: '',
    condition: '',
    medication: '',
    useSameAddress: false, // ตัวแปรสำหรับเช็คว่าใช้ที่อยู่เดียวกับผู้ดูแลหรือไม่
    address: {
      houseNo: '',
      moo: '',
      soi: '',
      road: '',
      subdistrict: '',
      district: '',
      province: '',
      zipcode: ''
    }
  },
  pdpaConsent: false
});

// ฟังก์ชัน Sync ที่อยู่หากเลือก "ใช้ที่อยู่เดียวกับผู้ดูแล"
watch(() => formData.dependent.useSameAddress, (isSame) => {
  if (isSame) {
    Object.assign(formData.dependent.address, formData.caregiver.address);
  } else {
    // ล้างค่าหากกดยกเลิก
    Object.keys(formData.dependent.address).forEach(key => {
      formData.dependent.address[key] = '';
    });
  }
});

// ตรวจจับหากผู้ดูแลแก้ไขที่อยู่ ให้ฝั่งผู้มีภาวะพึ่งพิงอัปเดตตามแบบ Real-time (ถ้าติ๊กเลือกไว้)
watch(() => formData.caregiver.address, (newAddress) => {
  if (formData.dependent.useSameAddress) {
    Object.assign(formData.dependent.address, newAddress);
  }
}, { deep: true });

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
      
      <!-- ==================================
           การ์ดที่ 1: ข้อมูลผู้ดูแล 
      =================================== -->
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

        <!-- Section: ที่อยู่ผู้ดูแล -->
        <div class="address-section">
          <h3>ที่อยู่ปัจจุบัน (ผู้ดูแล)</h3>
          
          <div class="grid-row grid-3-col">
            <div class="input-group">
              <label>บ้านเลขที่ <span class="req">*</span></label>
              <input type="text" v-model="formData.caregiver.address.houseNo" required placeholder="ตัวอย่าง: 123/4">
            </div>
            <div class="input-group">
              <label>หมู่ที่</label>
              <input type="text" v-model="formData.caregiver.address.moo" placeholder="ตัวอย่าง: 9">
            </div>
            <div class="input-group">
              <label>ซอย</label>
              <input type="text" v-model="formData.caregiver.address.soi" placeholder="ระบุซอย (ถ้ามี)">
            </div>
          </div>

          <div class="grid-row grid-3-col">
            <div class="input-group">
              <label>ถนน</label>
              <input type="text" v-model="formData.caregiver.address.road" placeholder="ระบุถนน (ถ้ามี)">
            </div>
            <div class="input-group">
              <label>ตำบล / แขวง <span class="req">*</span></label>
              <input type="text" v-model="formData.caregiver.address.subdistrict" required placeholder="ตำบล">
            </div>
            <div class="input-group">
              <label>อำเภอ / เขต <span class="req">*</span></label>
              <input type="text" v-model="formData.caregiver.address.district" required placeholder="อำเภอ">
            </div>
          </div>

          <div class="grid-row grid-2-col">
            <div class="input-group">
              <label>จังหวัด <span class="req">*</span></label>
              <input type="text" v-model="formData.caregiver.address.province" required placeholder="จังหวัด">
            </div>
            <div class="input-group">
              <label>รหัสไปรษณีย์ <span class="req">*</span></label>
              <input type="text" v-model="formData.caregiver.address.zipcode" required inputmode="numeric" pattern="[0-9]{5}" placeholder="ตัวเลข 5 หลัก">
            </div>
          </div>
        </div>
      </div>

      <!-- ==================================
           การ์ดที่ 2: ข้อมูลผู้มีภาวะพึ่งพิง
      =================================== -->
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

        <!-- Section: ที่อยู่ผู้พึ่งพิง -->
        <div class="address-section">
          <div class="address-header-flex">
            <h3>ที่อยู่ปัจจุบัน (ผู้มีภาวะพึ่งพิง)</h3>
            <label class="same-address-checkbox">
              <input type="checkbox" v-model="formData.dependent.useSameAddress">
              <span>ใช้ที่อยู่เดียวกับผู้ดูแล</span>
            </label>
          </div>
          
          <div class="grid-row grid-3-col">
            <div class="input-group">
              <label>บ้านเลขที่ <span class="req">*</span></label>
              <input type="text" v-model="formData.dependent.address.houseNo" required :disabled="formData.dependent.useSameAddress">
            </div>
            <div class="input-group">
              <label>หมู่ที่</label>
              <input type="text" v-model="formData.dependent.address.moo" :disabled="formData.dependent.useSameAddress">
            </div>
            <div class="input-group">
              <label>ซอย</label>
              <input type="text" v-model="formData.dependent.address.soi" :disabled="formData.dependent.useSameAddress">
            </div>
          </div>

          <div class="grid-row grid-3-col">
            <div class="input-group">
              <label>ถนน</label>
              <input type="text" v-model="formData.dependent.address.road" :disabled="formData.dependent.useSameAddress">
            </div>
            <div class="input-group">
              <label>ตำบล / แขวง <span class="req">*</span></label>
              <input type="text" v-model="formData.dependent.address.subdistrict" required :disabled="formData.dependent.useSameAddress">
            </div>
            <div class="input-group">
              <label>อำเภอ / เขต <span class="req">*</span></label>
              <input type="text" v-model="formData.dependent.address.district" required :disabled="formData.dependent.useSameAddress">
            </div>
          </div>

          <div class="grid-row grid-2-col">
            <div class="input-group">
              <label>จังหวัด <span class="req">*</span></label>
              <input type="text" v-model="formData.dependent.address.province" required :disabled="formData.dependent.useSameAddress">
            </div>
            <div class="input-group">
              <label>รหัสไปรษณีย์ <span class="req">*</span></label>
              <input type="text" v-model="formData.dependent.address.zipcode" required inputmode="numeric" pattern="[0-9]{5}" :disabled="formData.dependent.useSameAddress">
            </div>
          </div>
        </div>
      </div>

      <!-- ==================================
           การ์ดที่ 3: ความยินยอม
      =================================== -->
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
/* สไตล์เดิมที่ออกแบบมาตาม WCAG 2.2 และ Fitts's Law ยังคงไว้ครบถ้วน */

.research-form-container {
    max-width: 800px;
    margin: 0 auto;
    padding: 20px 15px 60px;
    font-family: 'Sarabun', sans-serif;
    color: #222222;
}

.form-header { text-align: center; margin-bottom: 30px; }

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

.form-header h1 { font-size: 1.8rem; margin: 0 0 10px 0; font-weight: 700; }
.form-header p { font-size: 1.1rem; color: #555555; margin: 0; }

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

.card-header h2 { font-size: 1.3rem; margin: 0; color: #111111; }

.grid-row { display: grid; gap: 20px; margin-bottom: 20px; }
.grid-1-col { grid-template-columns: 1fr; }
.grid-2-col { grid-template-columns: 1fr 1fr; }
.grid-3-col { grid-template-columns: 1fr 1fr 1fr; } /* เพิ่มสไตล์สำหรับที่อยู่ */

/* ดีไซน์ส่วนที่อยู่ */
.address-section {
    background-color: #FAFAFA;
    border: 1px dashed #CCCCCC;
    padding: 20px;
    border-radius: 8px;
    margin-top: 15px;
}

.address-section h3 {
    margin: 0 0 20px 0;
    font-size: 1.15rem;
    color: #444444;
}

.address-header-flex {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
    flex-wrap: wrap;
    gap: 10px;
}

.address-header-flex h3 { margin: 0; }

/* Checkbox ให้เลือกที่อยู่เดียวกัน */
.same-address-checkbox {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    background-color: #E8F5E9;
    padding: 8px 16px;
    border-radius: 20px;
    cursor: pointer;
    font-weight: 600;
    color: #2E7D32;
    transition: background-color 0.2s;
}

.same-address-checkbox:hover { background-color: #C8E6C9; }
.same-address-checkbox input { width: 18px; height: 18px; accent-color: #00B900; cursor: pointer; }

.input-group label { 
    display: block; 
    font-size: 1.1rem; 
    font-weight: 600; 
    margin-bottom: 8px; 
}

.req { color: #E53935; font-weight: bold; }

input[type="text"], input[type="tel"], input[type="password"], input[type="date"], select, textarea { 
    width: 100%; 
    min-height: 48px; 
    padding: 12px 15px; 
    border: 2px solid #DDDDDD; 
    border-radius: 8px; 
    font-size: 1.1rem; 
    background-color: #FFFFFF; 
    box-sizing: border-box; 
    transition: all 0.3s ease;
    color: #222222;
}

input:focus, select:focus, textarea:focus { 
    outline: none; 
    border-color: #00B900; 
    background-color: #FFFFFF; 
    box-shadow: 0 0 0 4px rgba(0, 185, 0, 0.15); 
}

/* หาก Input ถูก Disable (ตอนติ๊กใช้ที่อยู่เดียวกับผู้ดูแล) */
input:disabled {
    background-color: #EEEEEE;
    color: #777777;
    cursor: not-allowed;
    border-color: #CCCCCC;
}

select { cursor: pointer; appearance: auto; }

.pdpa-card { background-color: #F4FBFC; border: 1px solid #BCE3EB; }

.checkbox-wrapper { display: flex; align-items: flex-start; gap: 15px; cursor: pointer; }
.checkbox-wrapper input[type="checkbox"] { 
    width: 24px; 
    height: 24px; 
    margin-top: 2px; 
    cursor: pointer; 
    accent-color: #00B900;
}
.checkbox-content strong { display: block; font-size: 1.1rem; margin-bottom: 5px; color: #111; }
.checkbox-content p { font-size: 1rem; color: #555; line-height: 1.5; margin: 0; }

.form-actions { text-align: center; margin-top: 40px; }
.btn-submit { 
    background-color: #00B900; 
    color: #FFFFFF; 
    border: none; 
    min-height: 56px; 
    padding: 0 40px; 
    font-size: 1.25rem; 
    font-weight: bold; 
    border-radius: 28px; 
    cursor: pointer; 
    width: 100%; 
    max-width: 400px; 
    box-shadow: 0 6px 12px rgba(0, 185, 0, 0.2);
    transition: transform 0.1s, background-color 0.2s; 
}
.btn-submit:active { transform: scale(0.98); }
.btn-submit:hover { background-color: #009900; }

@media (max-width: 600px) { 
    .grid-2-col, .grid-3-col { grid-template-columns: 1fr; gap: 15px; } 
    .form-card { padding: 20px; border-radius: 8px; }
    .research-form-container { padding: 15px 10px; }
    .address-header-flex { flex-direction: column; align-items: flex-start; }
}
</style>