# Shop (Lottery Online)

# About

This project is the backend for Lottery Online.

....

# Flow

## 1. Search lottery

Input : 6 digit for search. (\_ \_ \_ \_ \_ \_)

Filter: ทั้งหมด / ชุด 2 ใบ / ชุด 3 ใบ / ชุด 4 ใบ (default: ทั้งหมด)

Note: การที่จะมี lottery ขึ้นมาต้องมีการ add ร้านค้าอย่างน้อย 1 ร้าน ก็จะสามารถหา lottery ทั้งหมดที่มีใน platform ได้ โดยถ้าปัดไปทางขวา `ข้อมูลผู้ขาย` จะเป็นรายละเอียดของร้านค้าที่เปิดล่าสุด

#### 1.1 lotLotteryCurrent:

ดึงข้อมูล Lot of Lottery ล่าสุด แล้วนำ LotID ไปใช้เพื่อ Filter Lottery ต่อ

- Query

```javascript
query {
  lotLotteryCurrent {
    lotID
    lotDate
    lotDescription
  }
}
```

#### 1.2 userShopLatest:

ถ้าหาก Login แล้วนำ `userID` ไปดึงข้อมูล `shopLatest` จาก `userInfo`

\*Note `shopLatest` = `shopID`

- Query

```javascript
query($userID: ID!) {
  userInfo(userID: $userID)  {
    roleType
  	userID
    phoneNumber
    bankCompanyAbbreviation
    bankNumber
    name
    surname
    shopLatest
    shopFriend {
      userID
      shopID
    }
    permissionAdminList
    shopID
    hasShop
    isOpen
    isSoldOut
    isVerifyOTP
  }
}
```

- Query Variables

```json
{
  "userID": "donut"
}
```

#### 1.3 searchLottery

สามารถ Filter โดยไม่ต้องมี `roleType` ก็ไก้

- Query

```javascript
query($input: FilterLotteryInput!) {
  lotteryAvailable(input: $input)  {
    lotID
    lotteryUniqueList {
      lotteryNo
      amount
      locationImg {
        filename
        full
        mini
      }
      lotterySeriesList{
        lotteryID
        lotterySeries
      }
    }
    nextPageToken
    limit
  }
}
```

- Query Variables

input FilterLotteryInput {}

```json
{
  "input": {
    "lotDateID": "2021-05-16",
    "digit1": "1",
    "digit2": "2",
    "digit3": "3",
    "digit4": "4",
    "digit5": "5",
    "digit6": "6",
    "isSoldOut": false,
    "all": true,
    "double": false,
    "triple": false,
    "quadruple": false,
    "nextPageToken": "",
    "limit": -1
  }
}
```

#### 1.4 shopLatestInfo

ถ้าหาก Login แล้วนำ `shopLatest.userID` ไปดึงข้อมูล `shopInfo` จาก `userInfo`

:: ส่วนตรวจสอบสถานะว่าร้านค้าเปิด คือ `isOpen` - `True:เปิดร้่าน`, `False:ปิดร้าน`

:: ส่วนตรวจสอบสถานะว่าร้านค้า lottery หมดแผง หรือไม่ คือ `isSoldOut` - `True:หมดแผง`, `False:ไม่หมดแผง`

:: ส่วนตรวจสอบสถานะว่าร้านค้าได้่รับการยืนยันตัวตนแล้ว คือ `isVerifyOTP` - `True:ยืนยันตัวตนแล้ว`, `False:ยังไม่ได้รับการยืนยันตัวตนแล้ว`

- Query

```javascript
query($userID: ID!) {
  userInfo(userID: $userID)  {
    roleType
  	userID
    shopID
    shopInfo {
      shopName
      name
      surname
      email
      bankCompanyAbbreviation
      bankNumber
      socialFacebookID
      sociallineID
      officeHours {
        mon {
          startTime
          endTime
        }
        tue {
          startTime
          endTime
        }
        wed {
          startTime
          endTime
        }
        thu {
          startTime
          endTime
        }
        fri {
          startTime
          endTime
        }
        sat {
          startTime
          endTime
        }
        sun {
          startTime
          endTime
        }
      }
    }
    hasShop
    isOpen
    isSoldOut
    isVerifyOTP
  }
}
```

- Query Variables

```json
{
  "userID": "donut"
}
```

![searchLottery](./images/searchLottery.png)

- _ทั้งหมด_ : show lottery ทุกใบโดยแบ่งทั้ง 1 ใบ, 2 ใบ, 3 ใบ, 4 ใบ, 5 ใบ (ถ้าเป็นชุด ก็ใส่ สัญลักษณ์ ไปในรูป lottery ด้วย)

- _ชุด 2 ใบ_ : show lottery เฉพาะชุด 2 ใบ (ถ้าเป็นชุด ก็ใส่ สัญลักษณ์ ไปในรูป lottery ด้วย)

- _ชุด 3 ใบ_ : show lottery เฉพาะชุด 3 ใบ (ถ้าเป็นชุด ก็ใส่ สัญลักษณ์ ไปในรูป lottery ด้วย)

- _ชุด 4 ใบ_ : show lottery เฉพาะชุด 4 ใบ (ถ้าเป็นชุด ก็ใส่ สัญลักษณ์ ไปในรูป lottery ด้วย)

ถ้าหากรัานปิด หรือ หมดแผงต้องแสดงดังนี้
![closeShop](./images/closeShop.png)

## 2. Add Shop

เพิ่มร้านค้าเพื่อที่จะซื้อ Lottery โดยการมีร้านค้าหลายๆ ร้านจะได้ benefit ที่แตกต่างกัน ได้แก่

- Credit ในการซื้อ Lottery
- Fee ค่าบริการที่แตกต่างกัน

ถ้าหาก Login แล้วนำ `userID` ไปดึงข้อมูล `shopFriend` จาก `userInfo`

![addShop](./images/addShop.png)

## 3. Complaint

แจ้งเรื่องร้องเรียน
![complaint](./images/complaint.png)

## 4. Shopping Cart (Real Time)

เมื่อเลือก lottery ใส่ `ตะกร้า` ระบบจะต้องทำการ ตรวจสอบว่า lottery นั้นๆ มีการซื้อหรือยัง ? (Real-time) ซึ่งเราจะ `จอง lottery` ก็ต่อเมื่อ ไปถึงหน้า `แจ้งการชำระเงิน หรือ มี หมายเลขออเดอร์` ขึ้นแล้ว **ซึ่ง _Credit ของผู้ขายก็ยังไม่ถูกตัด_**

<span style="color:red">\* ถึง lottery ถ้าอยู่ใน Cart แล้ว ยังไม่ถือว่าจอง lottery นั้นแล้ว</span>

ในส่วนของเงื่อนไข หรือ bussiness logic ของ Choice ในการเลือกซื้อของจำนวนใบแต่ละแบบ ได้แก่

- Switch case #1: 6 ใบ

เลือกซื้อได้ `6,3,2` ใบ

- Switch case #1: 5 ใบ

เลือกซื้อได้ `5,3,2` ใบ

- Switch case #2: 4 ใบ

เลือกซื้อได้ `4,2` ใบ

- Switch case #3: 3 ใบ

เลือกซื้อได้ `3` ใบ

- Switch case #4: 2 ใบ

เลือกซื้อได้ `2` ใบ

- Switch case #5: 1 ใบ

เลือกซื้อได้ `1` ใบ

![cart](./images/cart.png)

## 5. Register & Login with OTP

...TODO... (Dev)

## 6. Confirm Payment

ถ้าหากเข้ามาถึงหน้า "แจ้งการชำระเงิน" แล้ว lottery ทั้งหมดจะถูกจอง แล้วจะมีการนับเวลาถอยหลังเป็นเวลา 1 ชั่วโมง และจะตัด Credit ของผู้ขาย คนนั้นๆ

![confirmPayment](./images/confirmPayment.png)

## 7. Safe

ตู้เซฟสมาชิก จะมองเห็นในมุมมองของประวัติแบบต่างๆ

- งวดปัจจุบัน
- ประวัติการสั่งซื้อทั้งหมด
- ประวัติการถูกรางวัล
- ข้อมูลสมาชิก

### 7.1 งวดปัจจุบัน

สถานะ ของ Order

- **ยังไม่ชำระเงิน** <span style="color:red">(สีแดง)</span>
- **หมดเวลา** <span style="color:gray">(สีเทา)</span>
- **รอตรวจสอบการชำระเงิน** <span style="color:orange">(สีส้ม)</span>
- **ยกเลิก** <span style="color:red">(สีแดง)</span>
- **การสั่งซื้อเสร็จสมบูรณ์** <span style="color:green">(สีเขียว)</span>

![safeStatusOrder](./images/safeStatusOrder.png)

สถานะ ของ Lottery

- ไม่ถูกรางวัล <span style="color:red">(สีแดง)</span>
- ถูกรางวัล <span style="color:green">(สีเขียว)</span>
- ไม่ได้ชำระเงิน <span style="color:gray">(สีเทา)</span>

![safeStatusLottery](./images/safeStatusLottery.png)

### 7.2 ประวัติการถูกรางวัล

สถานะ ของ Lottery ที่ถูกรางวัล

- สำเร็จ
- รอมารับ

![rewardStatus](./images/rewardStatus.png)
