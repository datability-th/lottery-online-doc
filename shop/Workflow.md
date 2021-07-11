# Shop (Lottery Online)

# About

....

# Flow

## 1. Search lottery

Input : 6 digit for search. (\_ \_ \_ \_ \_ \_)

Filter: ทั้งหมด / ชุด 2 ใบ / ชุด 3 ใบ / ชุด 4 ใบ (default: ทั้งหมด)

Note: การที่จะมี lottery ขึ้นมาต้องมีการ add ร้านค้าอย่างน้อย 1 ร้าน ก็จะสามารถหา lottery ทั้งหมดที่มีใน platform ได้ โดยถ้าปัดไปทางขวา `ข้อมูลผู้ขาย` จะเป็นรายละเอียดของร้านค้าที่เปิดล่าสุด

![searchLottery](./images/searchLottery.png)

- _ทั้งหมด_ : show lottery ทุกใบโดยแบ่งทั้ง 1 ใบ, 2 ใบ, 3 ใบ, 4 ใบ, 5 ใบ (ถ้าเป็นชุด ก็ใส่ สัญลักษณ์ ไปในรูป lottery ด้วย)

- _ชุด 2 ใบ_ : show lottery เฉพาะชุด 2 ใบ (ถ้าเป็นชุด ก็ใส่ สัญลักษณ์ ไปในรูป lottery ด้วย)

- _ชุด 3 ใบ_ : show lottery เฉพาะชุด 3 ใบ (ถ้าเป็นชุด ก็ใส่ สัญลักษณ์ ไปในรูป lottery ด้วย)

- _ชุด 4 ใบ_ : show lottery เฉพาะชุด 4 ใบ (ถ้าเป็นชุด ก็ใส่ สัญลักษณ์ ไปในรูป lottery ด้วย)

## 2. Add Shop

เพิ่มร้านค้าเพื่อที่จะซื้อ Lottery โดยการมีร้านค้าหลายๆ ร้านจะได้ benefit ที่แตกต่างกัน ได้แก่

- Credit ในการซื้อ Lottery
- Fee ค่าบริการที่แตกต่างกัน

![addShop](./images/addShop.png)

## 3. ติดต่อเรา

...TODO... (Customer)

## 4. Shopping Cart (Real Time)

เมื่อเลือก lottery ใส่ `ตะกร้า` ระบบจะต้องทำการ ตรวจสอบว่า lottery นั้นๆ มีการซื้อหรือยัง ? (Real-time) ซึ่งเราจะ `จอง lottery` ก็ต่อเมื่อ ไปถึงหน้า `แจ้งการชำระเงิน หรือ มี หมายเลขออเดอร์` ขึ้นแล้ว

ในส่วนของเงื่อนไข หรือ bussiness logic ของ Choice ในการเลือกซื้อของจำนวนใบแต่ละแบบ ได้แก่

- Switch case #1: 5 ใบ

...TODO... (Customer)

- Switch case #2: 4 ใบ

...TODO... (Customer)

- Switch case #3: 3 ใบ

...TODO... (Customer)

- Switch case #4: 2 ใบ

...TODO... (Customer)

- Switch case #5: 1 ใบ

...TODO... (Customer)

![cart](./images/cart.png)
