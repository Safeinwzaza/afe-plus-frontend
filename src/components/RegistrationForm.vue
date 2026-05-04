<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ลงทะเบียนข้อมูล - AFE Plus</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

<div class="organized-form-container">
    <div class="form-header">
        <h1>ลงทะเบียนระบบ AFE Plus</h1>
        <p>กรุณากรอกข้อมูลเพื่อใช้ในการดูแลและแจ้งเตือน</p>
    </div>

    <form id="afePlusForm">
        
        <!-- ========================================== -->
        <!-- ส่วนที่ 1: ข้อมูลผู้ดูแล (Caregiver) -->
        <!-- ========================================== -->
        <div class="form-section">
            <h3 class="section-title">ส่วนที่ 1: ข้อมูลผู้ดูแล (Caregiver)</h3>
            
            <div class="grid-row grid-2-col">
                <div class="input-group">
                    <label for="cg-firstname">ชื่อ <span class="req">*</span></label>
                    <input type="text" id="cg-firstname" required placeholder="ชื่อจริง">
                </div>
                <div class="input-group">
                    <label for="cg-lastname">นามสกุล <span class="req">*</span></label>
                    <input type="text" id="cg-lastname" required placeholder="นามสกุล">
                </div>
            </div>

            <div class="grid-row grid-2-col">
                <div class="input-group">
                    <label for="cg-password">รหัสผ่าน <span class="req">*</span></label>
                    <input type="password" id="cg-password" required minlength="6" placeholder="รหัสผ่านอย่างน้อย 6 ตัวอักษร">
                </div>
                <div class="input-group">
                    <label for="cg-confirm-password">ยืนยันรหัสผ่าน <span class="req">*</span></label>
                    <input type="password" id="cg-confirm-password" required placeholder="กรอกรหัสผ่านอีกครั้ง">
                </div>
            </div>

            <div class="grid-row grid-2-col">
                <div class="input-group">
                    <label for="cg-pin">รหัส PIN (4 หลัก) <span class="req">*</span></label>
                    <input type="password" id="cg-pin" required pattern="[0-9]{4}" maxlength="4" placeholder="ตัวเลข 4 หลัก">
                </div>
                <div class="input-group">
                    <label for="cg-phone">เบอร์โทรศัพท์ <span class="req">*</span></label>
                    <input type="tel" id="cg-phone" required pattern="[0-9]{10}" placeholder="08XXXXXXXX">
                </div>
            </div>

            <div class="grid-row grid-1-col">
                <div class="input-group">
                    <label for="cg-address">ที่อยู่ปัจจุบัน <span class="req">*</span></label>
                    <textarea id="cg-address" required rows="2" placeholder="บ้านเลขที่, หมู่, ซอย, ถนน, ตำบล, อำเภอ, จังหวัด, รหัสไปรษณีย์"></textarea>
                </div>
            </div>
        </div>

        <!-- ========================================== -->
        <!-- ส่วนที่ 2: ข้อมูลผู้มีภาวะพึ่งพิง (Dependent) -->
        <!-- ========================================== -->
        <div class="form-section">
            <h3 class="section-title">ส่วนที่ 2: ข้อมูลผู้มีภาวะพึ่งพิง (Dependent)</h3>
            
            <div class="grid-row grid-2-col">
                <div class="input-group">
                    <label for="dp-firstname">ชื่อ <span class="req">*</span></label>
                    <input type="text" id="dp-firstname" required placeholder="ชื่อจริง">
                </div>
                <div class="input-group">
                    <label for="dp-lastname">นามสกุล <span class="req">*</span></label>
                    <input type="text" id="dp-lastname" required placeholder="นามสกุล">
                </div>
            </div>

            <div class="grid-row grid-2-col">
                <div class="input-group">
                    <label for="dp-dob">วัน/เดือน/ปีเกิด <span class="req">*</span></label>
                    <input type="date" id="dp-dob" required>
                </div>
                <div class="input-group">
                    <label for="dp-gender">เพศ <span class="req">*</span></label>
                    <select id="dp-gender" required>
                        <option value="" disabled selected>เลือกเพศ</option>
                        <option value="ชาย">ชาย</option>
                        <option value="หญิง">หญิง</option>
                        <option value="อื่นๆ">อื่นๆ</option>
                    </select>
                </div>
            </div>

            <div class="grid-row grid-2-col">
                <div class="input-group">
                    <label for="dp-marital">สถานภาพการสมรส</label>
                    <select id="dp-marital">
                        <option value="" disabled selected>เลือกสถานภาพ</option>
                        <option value="โสด">โสด</option>
                        <option value="สมรส">สมรส</option>
                        <option value="หย่าร้าง">หย่าร้าง</option>
                        <option value="หม้าย">หม้าย</option>
                    </select>
                </div>
                <div class="input-group">
                    <label for="dp-phone">เบอร์โทรศัพท์ (ถ้ามี)</label>
                    <input type="tel" id="dp-phone" pattern="[0-9]{10}" placeholder="08XXXXXXXX">
                </div>
            </div>

            <div class="grid-row grid-1-col">
                <div class="input-group">
                    <label for="dp-address">ที่อยู่ปัจจุบัน <span class="req">*</span></label>
                    <textarea id="dp-address" required rows="2" placeholder="บ้านเลขที่, หมู่, ซอย, ถนน, ตำบล, อำเภอ, จังหวัด, รหัสไปรษณีย์"></textarea>
                </div>
            </div>

            <div class="grid-row grid-2-col">
                <div class="input-group">
                    <label for="dp-condition">โรคประจำตัว</label>
                    <textarea id="dp-condition" rows="3" placeholder="ระบุโรคประจำตัว (ถ้ามี)"></textarea>
                </div>
                <div class="input-group">
                    <label for="dp-medication">ยาที่ใช้ประจำ</label>
                    <textarea id="dp-medication" rows="3" placeholder="ระบุชื่อยาและวิธีการรับประทาน (ถ้ามี)"></textarea>
                </div>
            </div>
        </div>

        <!-- ========================================== -->
        <!-- ส่วนที่ 3: PDPA -->
        <!-- ========================================== -->
        <div class="form-section pdpa-section">
            <label class="checkbox-wrapper">
                <input type="checkbox" id="pdpa-consent" required>
                <span class="checkbox-text">
                    ข้าพเจ้ายินยอมให้ระบบเก็บบันทึกข้อมูลเพื่อใช้ในการแจ้งเตือนและดูแลตามนโยบายความเป็นส่วนตัว <span class="req">*</span>
                </span>
            </label>
        </div>

        <div class="form-actions">
            <button type="submit" class="btn-submit">ลงทะเบียน</button>
        </div>

    </form>
</div>

<script src="script.js"></script>
</body>
</html>