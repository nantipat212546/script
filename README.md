# **SOMSOR — Full MVC Structure & Module Communication**

ผมจะอธิบายโดยยึดจากไฟล์ **SOMSOR_MVC.zip** ที่เราสร้างไว้จริง ครอบคลุมว่าแต่ละ Module มีอะไรอยู่ภายใน ทำหน้าที่อะไร เรียกใช้งานกันอย่างไร และรับ–ส่งข้อมูลอะไรบ้าง

สิ่งสำคัญคือ SOMSOR ใช้ **3-Tier Architecture ร่วมกับ MVC** โดยมีลำดับการสื่อสารหลักดังนี้

```
Frontend (HTML / CSS / JavaScript)
          ↕ HTTP / JSON
Express Routes
          ↓
Authentication & Authorization Middleware
          ↓
Controllers (Business Logic)
          ↕
Models / Database Access
          ↕ SQL
SQLite Database
```

ในเวอร์ชันปัจจุบัน Controllers บางส่วนติดต่อ SQLite โดยตรง ดังนั้นแผนภาพด้านล่างจะสะท้อนโครงสร้างจริง ไม่ได้สมมติว่าทุกคำสั่ง SQL ผ่าน Model ทั้งหมด

## **1. โครงสร้างไฟล์ของระบบ**

```
SOMSOR_MVC/
│
├── server.js                 # Entry Point
├── package.json              # Dependencies
│
├── config/
│   ├── database.js           # SQLite Connection
│   └── secret.js             # JWT Secret
│
├── routes/
│   ├── authRoutes.js
│   ├── requestRoutes.js
│   ├── categoryRoutes.js
│   ├── technicianRoutes.js
│   └── dashboardRoutes.js
│
├── controllers/
│   ├── authController.js
│   ├── requestController.js
│   ├── categoryController.js
│   ├── technicianController.js
│   └── dashboardController.js
│
├── models/
│   ├── userModel.js
│   ├── requestModel.js
│   ├── categoryModel.js
│   ├── technicianModel.js
│   └── setup.js
│
├── middleware/
│   └── auth.js
│
├── utils/
│   └── http.js
│
└── public/
    ├── index.html
    ├── app.js
    └── style.css
```

## **2. ความรับผิดชอบของแต่ละ Module**

| Module          | ภายในมีอะไร                                   | รับข้อมูลจาก          | ส่งข้อมูลไป                            |
| --------------- | --------------------------------------------- | --------------------- | -------------------------------------- |
| **Frontend**    | UI, ฟอร์ม, JavaScript                         | ผู้ใช้ / API Response | HTTP Request ไป Routes                 |
| **Routes**      | API Endpoint Mapping                          | Frontend              | Middleware และ Controllers             |
| **Middleware**  | JWT Verification, Role Checking               | Routes / HTTP Headers | `req.user` และ `next()` หรือ Error     |
| **Controllers** | Validation, Business Logic, Response          | Routes / Middleware   | Models, SQLite และ Frontend            |
| **Models**      | SQL Query Helpers, Data Access                | Controllers           | SQLite                                 |
| **Config**      | Database Connection, JWT Secret               | Server และ Modules    | Connection / Secret                    |
| **Database**    | 5 ตารางหลัก                                   | Models / Controllers  | Query Results                          |
| **Utils**       | Error Response, Input Cleaning, Error Wrapper | Controllers           | ข้อมูลที่ผ่านการจัดรูปแบบ / HTTP Error |

## **3. แผนภาพความสัมพันธ์ระหว่าง Modules**

\#chatgpt-mermaid-\_r_hv\_{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;fill:rgb(13, 13, 13);}@keyframes edge-animation-frame{from{stroke-dashoffset:0;}}@keyframes dash{to{stroke-dashoffset:0;}}#chatgpt-mermaid-\_r_hv\_ .edge-animation-slow{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 50s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-\_r_hv\_ .edge-animation-fast{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 20s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-\_r_hv\_ .error-icon{fill:rgb(243, 243, 243);}#chatgpt-mermaid-\_r_hv\_ .error-text{fill:rgb(13, 13, 13);stroke:rgb(13, 13, 13);}#chatgpt-mermaid-\_r_hv\_ .edge-thickness-normal{stroke-width:1px;}#chatgpt-mermaid-\_r_hv\_ .edge-thickness-thick{stroke-width:3.5px;}#chatgpt-mermaid-\_r_hv\_ .edge-pattern-solid{stroke-dasharray:0;}#chatgpt-mermaid-\_r_hv\_ .edge-thickness-invisible{stroke-width:0;fill:none;}#chatgpt-mermaid-\_r_hv\_ .edge-pattern-dashed{stroke-dasharray:3;}#chatgpt-mermaid-\_r_hv\_ .edge-pattern-dotted{stroke-dasharray:2;}#chatgpt-mermaid-\_r_hv\_ .marker{fill:rgb(143, 143, 143);stroke:rgb(143, 143, 143);}#chatgpt-mermaid-\_r_hv\_ .marker.cross{stroke:rgb(143, 143, 143);}#chatgpt-mermaid-\_r_hv\_ svg{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;}#chatgpt-mermaid-\_r_hv\_ p{margin:0;}#chatgpt-mermaid-\_r_hv\_ .label{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";color:rgb(13, 13, 13);}#chatgpt-mermaid-\_r_hv\_ .cluster-label text{fill:rgb(13, 13, 13);}#chatgpt-mermaid-\_r_hv\_ .cluster-label span{color:rgb(13, 13, 13);}#chatgpt-mermaid-\_r_hv\_ .cluster-label span p{background-color:transparent;}#chatgpt-mermaid-\_r_hv\_ .label text,#chatgpt-mermaid-\_r_hv\_ span{fill:rgb(13, 13, 13);color:rgb(13, 13, 13);}#chatgpt-mermaid-\_r_hv\_ .node rect,#chatgpt-mermaid-\_r_hv\_ .node circle,#chatgpt-mermaid-\_r_hv\_ .node ellipse,#chatgpt-mermaid-\_r_hv\_ .node polygon,#chatgpt-mermaid-\_r_hv\_ .node path{fill:rgb(222, 234, 251);stroke:rgb(83, 154, 248);stroke-width:1px;}#chatgpt-mermaid-\_r_hv\_ .rough-node .label text,#chatgpt-mermaid-\_r_hv\_ .node .label text,#chatgpt-mermaid-\_r_hv\_ .image-shape .label,#chatgpt-mermaid-\_r_hv\_ .icon-shape .label{text-anchor:middle;}#chatgpt-mermaid-\_r_hv\_ .node .katex path{fill:#000;stroke:#000;stroke-width:1px;}#chatgpt-mermaid-\_r_hv\_ .rough-node .label,#chatgpt-mermaid-\_r_hv\_ .node .label,#chatgpt-mermaid-\_r_hv\_ .image-shape .label,#chatgpt-mermaid-\_r_hv\_ .icon-shape .label{text-align:center;}#chatgpt-mermaid-\_r_hv\_ .node.clickable{cursor:pointer;}#chatgpt-mermaid-\_r_hv\_ .root .anchor path{fill:rgb(143, 143, 143)!important;stroke-width:0;stroke:rgb(143, 143, 143);}#chatgpt-mermaid-\_r_hv\_ .arrowheadPath{fill:rgb(143, 143, 143);}#chatgpt-mermaid-\_r_hv\_ .edgePath .path{stroke:rgb(143, 143, 143);stroke-width:1px;}#chatgpt-mermaid-\_r_hv\_ .flowchart-link{stroke:rgb(143, 143, 143);fill:none;}#chatgpt-mermaid-\_r_hv\_ .edgeLabel{background-color:rgb(252, 252, 252);text-align:center;}#chatgpt-mermaid-\_r_hv\_ .edgeLabel p{background-color:rgb(252, 252, 252);}#chatgpt-mermaid-\_r_hv\_ .edgeLabel rect{opacity:0.5;background-color:rgb(252, 252, 252);fill:rgb(252, 252, 252);}#chatgpt-mermaid-\_r_hv\_ .labelBkg{background-color:rgba(252, 252, 252, 0.5);}#chatgpt-mermaid-\_r_hv\_ .cluster rect{fill:rgb(243, 243, 243);stroke:rgba(0, 0, 0, 0.1);stroke-width:1px;}#chatgpt-mermaid-\_r_hv\_ .cluster text{fill:rgb(13, 13, 13);}#chatgpt-mermaid-\_r_hv\_ .cluster span{color:rgb(13, 13, 13);}#chatgpt-mermaid-\_r_hv\_ div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:12px;background:rgb(243, 243, 243);border:1px solid rgba(0, 0, 0, 0.1);border-radius:2px;pointer-events:none;z-index:100;}#chatgpt-mermaid-\_r_hv\_ .flowchartTitleText{text-anchor:middle;font-size:18px;fill:rgb(13, 13, 13);}#chatgpt-mermaid-\_r_hv\_ rect.text{fill:none;stroke-width:0;}#chatgpt-mermaid-\_r_hv\_ .icon-shape,#chatgpt-mermaid-\_r_hv\_ .image-shape{background-color:rgb(252, 252, 252);text-align:center;}#chatgpt-mermaid-\_r_hv\_ .icon-shape p,#chatgpt-mermaid-\_r_hv\_ .image-shape p{background-color:rgb(252, 252, 252);padding:2px;}#chatgpt-mermaid-\_r_hv\_ .icon-shape .label rect,#chatgpt-mermaid-\_r_hv\_ .image-shape .label rect{opacity:0.5;background-color:rgb(252, 252, 252);fill:rgb(252, 252, 252);}#chatgpt-mermaid-\_r_hv\_ .label-icon{display:inline-block;height:1em;overflow:visible;vertical-align:-0.125em;}#chatgpt-mermaid-\_r_hv\_ .node .label-icon path{fill:currentColor;stroke:revert;stroke-width:revert;}#chatgpt-mermaid-\_r_hv\_ .node .neo-node{stroke:rgb(83, 154, 248);}#chatgpt-mermaid-\_r_hv\_ [data-look="neo"].node rect,#chatgpt-mermaid-\_r_hv\_ [data-look="neo"].cluster rect,#chatgpt-mermaid-\_r_hv\_ [data-look="neo"].node polygon{stroke:url(#chatgpt-mermaid-\_r_hv\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_hv\_ [data-look="neo"].swimlane.cluster rect{filter:none;}#chatgpt-mermaid-\_r_hv\_ [data-look="neo"].node path{stroke:url(#chatgpt-mermaid-\_r_hv\_-gradient);stroke-width:1px;}#chatgpt-mermaid-\_r_hv\_ [data-look="neo"].node .outer-path{filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_hv\_ [data-look="neo"].node .neo-line path{stroke:rgb(83, 154, 248);filter:none;}#chatgpt-mermaid-\_r_hv\_ [data-look="neo"].node circle{stroke:url(#chatgpt-mermaid-\_r_hv\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_hv\_ [data-look="neo"].node circle .state-start{fill:#000000;}#chatgpt-mermaid-\_r_hv\_ [data-look="neo"].icon-shape .icon{fill:url(#chatgpt-mermaid-\_r_hv\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_hv\_ [data-look="neo"].icon-shape .icon-neo path{stroke:url(#chatgpt-mermaid-\_r_hv\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_hv\_ .node text{font-size:14px;font-weight:600;letter-spacing:normal;fill:rgb(0, 79, 153);}#chatgpt-mermaid-\_r_hv\_ .edgeLabels text{font-size:13px;font-weight:600;letter-spacing:-0.08px;fill:rgb(0, 79, 153);}#chatgpt-mermaid-\_r_hv\_ .node tspan[font-weight="normal"],#chatgpt-mermaid-\_r_hv\_ .edgeLabels tspan[font-weight="normal"]{font-weight:600;}#chatgpt-mermaid-\_r_hv\_ .edgeLabel .label rect{opacity:1;rx:13px;ry:13px;fill:rgb(245, 250, 255);stroke:rgb(206, 219, 229);stroke-width:1px;}#chatgpt-mermaid-\_r_hv\_ .node rect,#chatgpt-mermaid-\_r_hv\_ .node circle,#chatgpt-mermaid-\_r_hv\_ .node ellipse,#chatgpt-mermaid-\_r_hv\_ .node polygon,#chatgpt-mermaid-\_r_hv\_ .node path{fill:rgb(229, 243, 255);stroke:rgba(0, 0, 0, 0.1);stroke-width:1px;}#chatgpt-mermaid-\_r_hv\_ .node rect{rx:16px;ry:16px;}#chatgpt-mermaid-\_r_hv\_ .mermaid-decision .label-container{fill:rgb(245, 250, 255);stroke:rgb(206, 219, 229);stroke-dasharray:2,2;}#chatgpt-mermaid-\_r_hv\_ .edgePaths .flowchart-link{stroke:rgb(143, 143, 143);stroke-width:1px;stroke-linecap:round;stroke-linejoin:round;}#chatgpt-mermaid-\_r_hv\_ .marker{fill:rgb(143, 143, 143);stroke:rgb(143, 143, 143);}#chatgpt-mermaid-\_r_hv\_ :root{--mermaid-font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";}Users: Student / Admin /TechnicianVIEW: public/index.html,app.js, style.cssserver.js — Express ApplicationROUTES: auth / requests /categories / technicians /dashboardMIDDLEWARE: JWTAuthentication + RoleAuthorizationCONTROLLERS: Auth / Request/ Category / Technician /DashboardMODELS: User / Request /Category / TechnicianCONFIG: database.js / secret.jsSQLite: somsor.dbUTILS: fail / clean / wrapHTTP Request / JSON ResponseSQL บางส่วนในเวอร์ชันปัจจุบันHTTP Response

## **4. รายละเอียดภายในแต่ละ Module**

### **A. View — Frontend**

`public/`

ส่วนที่ผู้ใช้มองเห็นและโต้ตอบผ่าน Browser

| ไฟล์         | หน้าที่                                                     |
| ------------ | ----------------------------------------------------------- |
| `index.html` | โครงสร้างหน้าจอและฟอร์ม                                     |
| `style.css`  | รูปแบบ สี Layout และ Responsive UI                          |
| `app.js`     | จัดการการเข้าสู่ระบบ หน้าจอตามบทบาท เรียก API และแสดงข้อมูล |

ข้อมูลที่ส่งออก เช่น:

```
{
  "title": "ไฟห้องเรียนเสีย",
  "description": "ไฟกะพริบตลอดเวลา",
  "location": "อาคาร A ห้อง 305",
  "category_id": 1
}
```

Frontend ส่งข้อมูลนี้ไปยัง Backend ผ่าน `POST /api/v1/requests` พร้อม JWT ใน Authorization Header

### **B. Routes — API Routing**

Routes เป็นตัวกำหนดว่า HTTP Method และ URL ใดจะไปยัง Controller Handler ใด

| Route Module          | Endpoint หลัก                                                                | Controller |
| --------------------- | ---------------------------------------------------------------------------- | ---------- |
| `authRoutes.js`       | `/auth/register`, `/auth/login`, `/auth/me`                                  | Auth       |
| `requestRoutes.js`    | `/requests`, `/requests/:id`, `/requests/:id/assign`, `/requests/:id/status` | Request    |
| `categoryRoutes.js`   | `/categories`, `/categories/:id`                                             | Category   |
| `technicianRoutes.js` | `/technicians`                                                               | Technician |
| `dashboardRoutes.js`  | `/dashboard`                                                                 | Dashboard  |

ทุก Endpoint ข้างต้นอยู่ภายใต้ Prefix `/api/v1`

ในโค้ดจริง Routes เรียกชุด Handler เช่น `handler0`, `handler1` ที่ export จาก Controller ซึ่งแต่ละชุดมี Middleware และฟังก์ชันประมวลผลประกอบกัน

### **C. Middleware — Authentication & Authorization**

ไฟล์ `middleware/auth.js` มีฟังก์ชันหลัก 2 ตัว

`auth(req, res, next)`

- อ่าน JWT จาก `Authorization: Bearer <token>`
- ตรวจสอบลายเซ็นและอายุ Token
- ถ้าถูกต้อง นำข้อมูลผู้ใช้ใส่ใน `req.user`
- ถ้าไม่ถูกต้อง ส่ง `401 Unauthorized`

`roles(...allowed)`

- ตรวจสอบ `req.user.role`
- อนุญาตเฉพาะบทบาทที่กำหนด
- ถ้าไม่มีสิทธิ์ ส่ง `403 Forbidden`

ตัวอย่างการใช้งาน:

```
[auth, roles('admin'), controllerFunction]
```

หมายถึงต้องเข้าสู่ระบบและเป็น Admin ก่อนจึงดำเนินการต่อได้

### **D. Controllers — Business Logic**

`authController.js`

รับชื่อ อีเมล รหัสผ่าน และจัดการ Register/Login โดยใช้ `bcryptjs` สำหรับ Hash/Verify Password และ `jsonwebtoken` สำหรับสร้าง JWT

ส่งผลลัพธ์เป็นข้อมูลบัญชีผู้ใช้หรือ Token

`requestController.js`

จัดการวงจรชีวิตงานแจ้งซ่อมทั้งหมด ได้แก่ สร้างรายการ ดูรายการ ดูรายละเอียด มอบหมายช่าง เปลี่ยนสถานะ และยกเลิกรายการ

ตรวจสอบเงื่อนไข เช่น มอบหมายได้เฉพาะงาน `pending` และช่างต้องมีสถานะ `available`

`categoryController.js`

อ่าน เพิ่ม และแก้ไขประเภทงานซ่อม ตรวจสอบข้อมูลว่างและชื่อซ้ำ โดยสิทธิ์เพิ่ม/แก้ไขเป็นของ Admin

`technicianController.js`

ส่งรายชื่อช่าง พร้อมข้อมูลผู้ใช้ ความเชี่ยวชาญ และสถานะว่าง/ไม่ว่าง โดยให้ Admin เรียกดู

`dashboardController.js`

สรุปจำนวนรายการตามสถานะ โดยกรองข้อมูลตามบทบาท Student เห็นงานของตัวเอง Technician เห็นงานที่ได้รับมอบหมาย และ Admin เห็นทั้งหมด

### **E. Models — Data Access**

| Model                | ฟังก์ชัน / ข้อมูลสำคัญ              |
| -------------------- | ----------------------------------- |
| `userModel.js`       | `findByEmail()`, `findPublicById()` |
| `requestModel.js`    | `requestSQL`, `canSee()`            |
| `categoryModel.js`   | `all()`                             |
| `technicianModel.js` | `all()`                             |
| `setup.js`           | สร้างตาราง Index และข้อมูลเริ่มต้น  |

จุดที่ควรเข้าใจคือ `requestModel.js` ในเวอร์ชันนี้มี SQL สำหรับอ่านรายการและตรวจสอบสิทธิ์ แต่การสร้างงาน มอบหมายงาน และเปลี่ยนสถานะยังดำเนินการ SQL ใน `requestController.js` โดยตรง

### **F. Database — SQLite**

**somsor.db**

users

บัญชีผู้ใช้และบทบาท

requests

รายการแจ้งซ่อมและสถานะ

assignments

ประวัติการมอบหมายงาน

categories

ประเภทงานซ่อม

technicians

ข้อมูลช่างและสถานะ

ความสัมพันธ์สำคัญคือ `requests.user_id` เชื่อมกับ `users`, `requests.category_id` เชื่อมกับ `categories` และ `assignments` เชื่อมงานแจ้งซ่อมกับช่างผ่าน `request_id` และ `technician_id`

## **5. ตัวอย่างการสื่อสารจริง: Admin มอบหมายงานให้ช่าง**

SQLiteRequest ControllerAuth MiddlewareRequest RouteFrontendSQLiteRequest ControllerAuth MiddlewareRequest RouteFrontend#chatgpt-mermaid-\_r_ig\_{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;fill:rgb(13, 13, 13);}@keyframes edge-animation-frame{from{stroke-dashoffset:0;}}@keyframes dash{to{stroke-dashoffset:0;}}#chatgpt-mermaid-\_r_ig\_ .edge-animation-slow{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 50s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-\_r_ig\_ .edge-animation-fast{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 20s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-\_r_ig\_ .error-icon{fill:rgb(243, 243, 243);}#chatgpt-mermaid-\_r_ig\_ .error-text{fill:rgb(13, 13, 13);stroke:rgb(13, 13, 13);}#chatgpt-mermaid-\_r_ig\_ .edge-thickness-normal{stroke-width:1px;}#chatgpt-mermaid-\_r_ig\_ .edge-thickness-thick{stroke-width:3.5px;}#chatgpt-mermaid-\_r_ig\_ .edge-pattern-solid{stroke-dasharray:0;}#chatgpt-mermaid-\_r_ig\_ .edge-thickness-invisible{stroke-width:0;fill:none;}#chatgpt-mermaid-\_r_ig\_ .edge-pattern-dashed{stroke-dasharray:3;}#chatgpt-mermaid-\_r_ig\_ .edge-pattern-dotted{stroke-dasharray:2;}#chatgpt-mermaid-\_r_ig\_ .marker{fill:rgb(143, 143, 143);stroke:rgb(143, 143, 143);}#chatgpt-mermaid-\_r_ig\_ .marker.cross{stroke:rgb(143, 143, 143);}#chatgpt-mermaid-\_r_ig\_ svg{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;}#chatgpt-mermaid-\_r_ig\_ p{margin:0;}#chatgpt-mermaid-\_r_ig\_ .actor{stroke:rgb(83, 154, 248);fill:rgb(222, 234, 251);stroke-width:1;}#chatgpt-mermaid-\_r_ig\_ rect.actor.outer-path[data-look="neo"]{filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_ig\_ rect.note[data-look="neo"]{stroke:rgb(107, 198, 127);fill:rgb(243, 243, 243);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_ig\_ text.actor>tspan{fill:rgb(13, 13, 13);stroke:none;}#chatgpt-mermaid-\_r_ig\_ .actor-line{stroke:rgb(143, 143, 143);}#chatgpt-mermaid-\_r_ig\_ .innerArc{stroke-width:1.5;stroke-dasharray:none;}#chatgpt-mermaid-\_r_ig\_ .messageLine0{stroke-width:1.5;stroke-dasharray:none;stroke:rgb(143, 143, 143);}#chatgpt-mermaid-\_r_ig\_ .messageLine1{stroke-width:1.5;stroke-dasharray:2,2;stroke:rgb(143, 143, 143);}#chatgpt-mermaid-\_r_ig\_ [id$="-arrowhead"] path{fill:rgb(143, 143, 143);stroke:rgb(143, 143, 143);}#chatgpt-mermaid-\_r_ig\_ .sequenceNumber{fill:#707070;}#chatgpt-mermaid-\_r_ig\_ [id$="-sequencenumber"]{fill:rgb(143, 143, 143);}#chatgpt-mermaid-\_r_ig\_ [id$="-crosshead"] path{fill:rgb(143, 143, 143);stroke:rgb(143, 143, 143);}#chatgpt-mermaid-\_r_ig\_ .messageText{fill:rgb(13, 13, 13);stroke:none;}#chatgpt-mermaid-\_r_ig\_ .labelBox{stroke:rgba(0, 0, 0, 0.1);fill:rgb(252, 252, 252);filter:none;}#chatgpt-mermaid-\_r_ig\_ .labelText,#chatgpt-mermaid-\_r_ig\_ .labelText>tspan{fill:rgb(13, 13, 13);stroke:none;}#chatgpt-mermaid-\_r_ig\_ .loopText,#chatgpt-mermaid-\_r_ig\_ .loopText>tspan{fill:rgb(13, 13, 13);stroke:none;}#chatgpt-mermaid-\_r_ig\_ .sectionTitle,#chatgpt-mermaid-\_r_ig\_ .sectionTitle>tspan{fill:rgb(13, 13, 13);stroke:none;}#chatgpt-mermaid-\_r_ig\_ .loopLine{stroke-width:2px;stroke-dasharray:2,2;stroke:rgba(0, 0, 0, 0.1);fill:rgba(0, 0, 0, 0.1);}#chatgpt-mermaid-\_r_ig\_ .note{stroke:rgb(107, 198, 127);fill:rgb(243, 243, 243);}#chatgpt-mermaid-\_r_ig\_ .noteText,#chatgpt-mermaid-\_r_ig\_ .noteText>tspan{fill:rgb(13, 13, 13);stroke:none;font-weight:normal;}#chatgpt-mermaid-\_r_ig\_ .activation0{fill:rgb(243, 243, 243);stroke:hsl(0, 0%, 85.2941176471%);}#chatgpt-mermaid-\_r_ig\_ .activation1{fill:rgb(243, 243, 243);stroke:hsl(0, 0%, 85.2941176471%);}#chatgpt-mermaid-\_r_ig\_ .activation2{fill:rgb(243, 243, 243);stroke:hsl(0, 0%, 85.2941176471%);}#chatgpt-mermaid-\_r_ig\_ .actorPopupMenu{position:absolute;}#chatgpt-mermaid-\_r_ig\_ .actorPopupMenuPanel{position:absolute;fill:rgb(222, 234, 251);box-shadow:0px 8px 16px 0px rgba(0,0,0,0.2);filter:drop-shadow(3px 5px 2px rgb(0 0 0 / 0.4));}#chatgpt-mermaid-\_r_ig\_ .actor-man circle,#chatgpt-mermaid-\_r_ig\_ line{fill:rgb(222, 234, 251);stroke-width:2px;}#chatgpt-mermaid-\_r_ig\_ g rect.rect{filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));stroke:rgb(83, 154, 248);}#chatgpt-mermaid-\_r_ig\_ .node .neo-node{stroke:rgb(83, 154, 248);}#chatgpt-mermaid-\_r_ig\_ [data-look="neo"].node rect,#chatgpt-mermaid-\_r_ig\_ [data-look="neo"].cluster rect,#chatgpt-mermaid-\_r_ig\_ [data-look="neo"].node polygon{stroke:url(#chatgpt-mermaid-\_r_ig\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_ig\_ [data-look="neo"].swimlane.cluster rect{filter:none;}#chatgpt-mermaid-\_r_ig\_ [data-look="neo"].node path{stroke:url(#chatgpt-mermaid-\_r_ig\_-gradient);stroke-width:1px;}#chatgpt-mermaid-\_r_ig\_ [data-look="neo"].node .outer-path{filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_ig\_ [data-look="neo"].node .neo-line path{stroke:rgb(83, 154, 248);filter:none;}#chatgpt-mermaid-\_r_ig\_ [data-look="neo"].node circle{stroke:url(#chatgpt-mermaid-\_r_ig\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_ig\_ [data-look="neo"].node circle .state-start{fill:#000000;}#chatgpt-mermaid-\_r_ig\_ [data-look="neo"].icon-shape .icon{fill:url(#chatgpt-mermaid-\_r_ig\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_ig\_ [data-look="neo"].icon-shape .icon-neo path{stroke:url(#chatgpt-mermaid-\_r_ig\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_ig\_ :root{--mermaid-font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";}alt[งาน pending และช่าง available][งานถูกมอบหมายแล้วหรือช่างไม่ว่าง]Adminเลือกงานและช่างPOST /api/v1/requests/:id/assignตรวจสอบ JWT และ Admin Roleส่ง req.user และ Request BodySELECT งานและข้อมูลช่างสถานะงานและสถานะช่างBEGIN TransactionINSERT assignmentsUPDATE requests = in_progressUPDATE technicians = busyCommit สำเร็จ200 มอบหมายสำเร็จ409 ConflictแสดงผลการมอบหมายAdmin

### **ข้อมูลที่รับและส่ง**

**Request**

```
POST /api/v1/requests/12/assign
Authorization: Bearer <JWT>
Content-Type: application/json
```

```
{
  "technician_id": 1,
  "note": "ตรวจสอบระบบไฟฟ้า"
}
```

**Response เมื่อสำเร็จ**

```
{
  "message": "มอบหมายสำเร็จ"
}
```

**ผลต่อฐานข้อมูล**

| ตาราง         | การเปลี่ยนแปลง            |
| ------------- | ------------------------- |
| `assignments` | เพิ่มข้อมูลการมอบหมาย     |
| `requests`    | `pending` → `in_progress` |
| `technicians` | `available` → `busy`      |

## **6. สรุปความสัมพันธ์ MVC ของ SOMSOR**

- **View:** รับข้อมูลผู้ใช้ แสดงผล และสื่อสารกับ REST API
- **Controller:** ตรวจสอบคำขอ บังคับใช้กฎของระบบ และกำหนด HTTP Response
- **Model:** เป็นส่วนหนึ่งของการเข้าถึงข้อมูลและตรวจสอบความสัมพันธ์ของข้อมูล
- **Routes + Middleware:** เป็นองค์ประกอบสนับสนุนการรับคำขอ การยืนยันตัวตน และการกำหนดสิทธิ์
- **SQLite:** จัดเก็บข้อมูลถาวรของระบบ

**สำหรับการนำเสนออาจารย์:** สามารถเรียกโครงสร้างนี้ว่า *MVC-based Express Architecture* ได้อย่างเหมาะสม แต่ไม่ควรอธิบายว่า Controllers ไม่ติดต่อ Database โดยตรง เพราะโค้ดปัจจุบันยังมีการเรียก SQL จาก Controller อยู่จริงครับ
