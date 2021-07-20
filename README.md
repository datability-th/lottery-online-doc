# Lottery Online Document

# About

....

# Database (Schema)

## 1. User

**Part #1** : Search a latest shop with `shopFriendLatest`.

สามารถค้นหา Shop ที่ล่าสุด ของลูกค้าเลือก เนื่องจากแต่ละ shop จะมี quota ไม่เท่ากันดังนั้นการเลือก shop จึงสำคัญ โดย Algorithm ต้องเอา `shopID` ไปค้นหาใน `Shop Table` โดยใช้ `shopInfo` ในการดูรายละเอียดของ Shop

**Part #2** : Search a list shop of user with `shopFriend`.

เมื่อค้นหา ShopFriend จะต้องไปดึงข้อมูลของ Quota จาก `shopQuota`

**Part #3** : Add a friend with `addShopFriend`.

เพิ่มเพื่อนกับร้านค้า โดยใช้ `shopInfo` เพื่อตรวจสอบว่ามีร้านค้านี้มั้ย ถ้ามีก็เพิ่มเข้าไปใน shopFriend ของ User เลย

### Backend Logical Return Value User Schema

```json
{
  "user": [
    {
      "roleType": "ลูกค้า", // PK ลูกค้า / ยี่ปั้ว / ซาปั้ว / สี่ปั้ว / โหงวปั้ว / admin
      "ีuserID": "1pfhes9biiur5lidlpi6cdpt5j", // SK
      "phoneNumber": "0962963233",
      "bankCompanyAbbreviation": "KBANK",
      "bankNumber": "72093208283",
      "name": "ภราดร",
      "surname": "ศรีชาพันธุ์",

      "shopLatest": "2xji1s", // phoneNumber
      "shopFriend": [
        {
          "ีuserID": "1pfhes9biiur5lidlpi6cdpt5j",
          "shopID": "2xji1s"
        },
        {
          "ีuserID": "1pfhes9biiur5lidlpi6cdpt5j",
          "shopID": "2xji1s"
        }
        // ...
      ],

      "permissionAdmin": [
        "delegator",
        "importLottery",
        "financeReport",
        "contentMarketing",
        "report",
        "createAdmin"
      ],

      "createdAt": "2021-07-16T10:13:32+00:00",
      "updatedAt": "2021-07-16T10:13:32+00:00"
    }
    // ...
  ]
}
```

Logically, a User

```typescript
type Query {
  shopFriendLatest(lotDate: Date!, phoneNumber: String, shopFriendPhoneNumber: String): Shop!
  shopFriend(lotDate: Date!, phoneNumber: String): [Shop]!
}

type Mutation {
  addShopFriend(input: AddLotteryInput!): User!
  // createLottery(input: CreateLotteryInput!): Lottery!
  // updateLottery(input: UpdateLotteryInput!): Lottery!
  // deleteLottery(input: DeleteLotteryInput!): Lottery!
}
```

## 2. Bank

```json
{
  "bank": [
    {
      "bankCompanyAbbreviation": "SCB", // PK
      "bankCompany": "ไทยพาณิชย์ (Siam Commercial Bank)"
    },
    {
      "bankCompanyAbbreviation": "KBANK",
      "bankCompany": "กสิกรไทย (Kasikornbank)"
    }
    // ...
  ]
}
```

## 3. Lot of Lottery

```json
{
  "lotLottery": [
    {
      "lotDate": "2021-05-16", // PK YYYY-MM-DD 16 พฤษภาคม 2564
      "lotDescription": "19" // SK งวดที่
    },
    {
      "lotDate": "2021-05-01", // PK YYYY-MM-DD1 พฤษภาคม 2564
      "lotDescription": "19" // SK งวดที่
    }
    // ...
  ]
}
```

## 4. Shop

**Part #1** : Search a information of shop with `shopInfo`.

สามารถค้นหาข้อมูล Shop

**Part #2** : List a shop with `shopInfo`.

รายการตัวแทน

### Backend Logical Return Value Shop Schema

```json
{
  "shop": [
    {
      // User
      "userID": "1pfhes9biiur5lidlpi6cdpt5j", // PK เบอร์โทร

      // Shop
      "shopID": "2xji1s", // SK รหัสตัวแทน
      "shopName": "ชื่อร้าน", // ชื่อร้าน
      "name": "ภราดร",
      "surname": "ศรีชาพันธุ์",
      "email": "donut@gmail.com",
      "idCard": "1100900984382", // เลขบัตรประชาชน
      "idCardImg": "https://s3.amazonaws.com/bucketname/foldername/imagename_idcard.jpg", // เลขบัตรประชาชน
      "upline": "<userID>", // Upline's Hierarchy Relation with `User Table`

      // Shop Bank
      "bankCompanyAbbreviation": "SCB", // ชื่อธนาคาร Relation with `Bank Table`
      "bankNumber": "72093208283", // เลขที่บัญชี

      // Social Media
      "socialFacebookID": "Facebook", // Facebook
      "sociallineID": "@lottoonline", // Line

      // Quota
      "quotaType": "โควต้าขายขาด", // ประเภทโควต้า - โควต้าขายขาด, โควต้าลอย
      "quotaClass": "โอนขาด", // รูปแบบเครดิต - โอนขาด, ไม่จำกัด
      "hasShop": true, // true: มี, false: ไม่มี

      "officeHours": {
        "mon": { "startTime": "08.00", "endTime": "17.00" },
        "tue": { "startTime": "08.00", "endTime": "17.00" },
        "wed": { "startTime": "08.00", "endTime": "17.00" },
        "thu": { "startTime": "08.00", "endTime": "17.00" },
        "fri": { "startTime": "08.00", "endTime": "17.00" },
        "sat": { "startTime": "08.00", "endTime": "17.00" },
        "sun": { "startTime": "08.00", "endTime": "17.00" }
      },

      "feeLottery": {
        "amountOne": 20,
        "amountTwo": 20,
        "amountThree": 40,
        "amountFour": 40,
        "amountFive": 40,
        "amountSix": 40,
        "amountSeven": 40,
        "amountEight": 40,
        "amountNine": 40,
        "amountTen": 40
      },

      // Status
      "isOpen": true, // true: Open, false: Close
      "isSoldOut": false, // true: หมดแผง, false: ขายต่อ
      "isVerified": true, // true: Verified, false: Not verified

      // Time
      "lastLogin": "2021-07-16T10:13:32+00:00", // Datetime
      "createdAt": "2021-07-16T10:13:32+00:00", // Datetime
      "updatedAt": "2021-07-16T10:13:32+00:00"
    }
    // ...
  ]
}
```

Logically, a Shop

```typescript
type Query {
  shopInfo(lotDate: Date!, phoneNumber: String): Shop!
  shopInfoList(lotDate: Date!, roleClass: String): [Shop]!
  shopInfoListHierarchy(lotDate: Date!, parentPhoneNumber: String): [Shop]!
}

type Mutation {
  createShop(input: CreateShopInput!): Shop!
  updateShop(input: UpdateShopInput!): Shop!
  deleteShop(input: DeleteShopInput!): Shop!
}
```

## 5. Quota of shop (Transaction)

**Part #1** : summary a total quota of shop with `shopQuota`.

```json
{
  "transactionQuota": [
    {
      "lotDate": "2021-05-16", // PK YYYY-MM-DD งวด 16 พฤษภาคม 2564
      "quota": 200.0,
      // Sender
      "shopSenderID": "2xji1s", // SK รหัสตัวแทน
      "nameSender": "ภราดร",
      "surnameSender": "ศรีชาพันธุ์",

      // Receiver
      "shopReceiverID": "2xji1s",
      "nameReceiver": "ภราดร",
      "surnameReceiver": "ศรีชาพันธุ์",

      "createdAt": "2021-07-16T10:13:32+00:00", // Datetime
      "updatedAt": "2021-07-16T10:13:32+00:00"
    }
    // ...
  ]
}
```

Logically, a Quota

```typescript
type Query {
  shopQuota(lotDate: Date!, phoneNumber: String): SummaryQuota!
}

type Mutation {
  // createLottery(input: CreateLotteryInput!): Lottery!
  // updateLottery(input: UpdateLotteryInput!): Lottery!
  // deleteLottery(input: DeleteLotteryInput!): Lottery!
}
```

## 6. Lottery

There are 4 main flows of the Project process.

#### 6.1 Lottery

**Part #1** : Search a lottery with `searchLottery`.

สามารถค้นหา lottery ที่ต้องการได้ตาม Digit รวมถึงถ้าเลขซ้ำกัน แต่คนละชุด (lotterySeries) ให้ Backend รวมเป็นชุดมาให้เลย

### Backend Logical Return Value Lottery Schema

```json
{
  "lottery": [
    {
      // งวดที่
      "lotDate": "2021-05-16", // PK YYYY-MM-DD งวด 16 พฤษภาคม 2564
      "lotteryID": "123456_10", // SK <LotteryNo_LotterySeries>
      "lotDescription": "19", // งวดที่
      "lotterySeries": "10", // ชุดที่

      // จัดเก็บที่ตู้หมายเลข
      "safe": {
        "safeNo": "", // จัดเก็บที่ตู้หมายเลข
        "safeGroupNo": "", // มัดที่
        "safeBookNo": "" // เล่มที่
      },

      // สลาก
      "lotteryNo": "123456",
      "digit1": "1",
      "digit2": "2",
      "digit3": "3",
      "digit4": "4",
      "digit5": "5",
      "digit6": "6",
      "locationImg": {
        "filename": "64-19-10-151022.jpg",
        "mini": "https://s3.amazonaws.com/bucketname/foldername/imagename_mini.jpg",
        "full": "https://s3.amazonaws.com/bucketname/foldername/imagename_full.jpg"
      },

      // ผู้ทำรายการ (ผู้นำ lottery เข้าสู่ระบบ)
      "importByAdmin": {
        "phoneNumber": "096254374", // เบอร์โทร
        "name": "ภราดร",
        "surname": "ศรีชาพันธุ์"
      },

      // ผู้นำเข้าฝากสลาก (ตัวแทน)
      "importBySeller": {
        "phoneNumber": "096254374", // เบอร์โทร
        "name": "ภราดร",
        "surname": "ศรีชาพันธุ์"
      },

      // Status
      "isSoldOut": false, // true:SOLD OUT, false: Available
      "isWon": false, // true:Won, false: Not won
      "orderNo": "", // Relative Order of lottery (Transaction) Table

      "getMoney": {
        "getType": true, // เลือกวิธีรับรางวัล- true: รับด้วยตนเองที่บริษัท, false: รับโดยวิธีการโอน (fee 2%)
        /*
        + isPaidStatus (รับโดยวิธีการโอน)
          - รอดำเนินการ
          - กรอกข้อมูลแล้ว
          - รอดำเนินการโอนเงิน
          - โอนเงินสำเร็จ
          - โอนเงินไม่สำเร็จ
          - รอตรวจสอบความถูกต้อง
        ----------------------------
        + isPaidStatus (รับด้วยตนเองที่บริษัท)
          - ยังไม่จัดเตรียม
          - จัดเตรียมแล้ว
          - ส่งมอบแล้ว
        */
        "isPaidStatus": "รอดำเนินการ",
        "meetDate": "2020-14-03T08:02:10",
        "idCardImg": "https://s3.amazonaws.com/bucketname/foldername/imagename_idcard.jpg", // เลขบัตรประชาชน
        "reward": 4000.0,
        "rewardNet": 3920.0
      },

      "createdAt": "2020-14-03T08:02:10",
      "updatedAt": "2020-14-03T08:02:10"
    }
    // ...
  ]
}
```

Logically, a LotDate has many Lottery's.

```typescript
type Query {
  searchLottery(lotDate: Date!, isSoldOut: Boolean, digit1: String, digit2: String, digit3: String, digit4: String, digit5: String, digit6: String, filter: { all: Boolean, double: Boolean, triple: Boolean, quadruple: Boolean }): [Lottery]!
}

type Mutation {
  // createLottery(input: CreateLotteryInput!): Lottery!
  // updateLottery(input: UpdateLotteryInput!): Lottery!
  // deleteLottery(input: DeleteLotteryInput!): Lottery!
}
```

## 7. Cart

```json
{
  "shoppingCart": [
    {
      "lotDate": "2021-05-16", // PK YYYY-MM-DD 16 พฤษภาคม 2564
      "orderNo": "11312624917052", // SK หมายเลขออเดอร์
      "amount": "", // จำนวน
      "price": "", // ราคา
      "fee": "", // ค่าบริการ

      "phoneNumber": "096254374", // เบอร์โทร
      "name": "",
      "surname": "",
      "note": "", // หมายเหตุ

      "buyerName": "",
      "buyerSurname": "",
      "buyerPhoneNumber": "",

      "sellerName": "",
      "sellerSurname": "",
      "sellerPhoneNumber": "",
      "sellerSociallineID": "@lottoonline", // Line

      // Status
      "statusOrder": "การสั่งซื้อเสร็จสมบูรณ์",

      // Slip
      "locationImg": {
        "filename": "64-19-10-151022.jpg",
        "slip": "https://s3.amazonaws.com/bucketname/foldername/imagename_full.jpg"
      },

      // Lottery
      "lottery": [
        {
          // งวดที่
          "lotDate": "2021-05-16", // YYYY-MM-DD งวด 16 พฤษภาคม 2564
          "lotteryID": "123456_10", // <LotteryNo_LotterySeries>
          "lotDescription": "19", // งวดที่
          "lotterySeries": "10", // ชุด

          // สลาก
          "lotteryNo": "123456",
          "locationImg": {
            "filename": "64-19-10-151022.jpg",
            "mini": "https://s3.amazonaws.com/bucketname/foldername/imagename_mini.jpg",
            "full": "https://s3.amazonaws.com/bucketname/foldername/imagename_full.jpg"
          },

          // Status
          "isSoldOut": false // true:SOLD OUT, false: Available
        }
        // ...
      ],

      "createdAt": "2020-14-03T08:02:10",
      "updatedAt": "2020-14-03T08:02:10"
    }
    // ...
  ]
}
```

## 8. Order of lottery (Transaction)

```json
{
  "transactionOrder": [
    {
      "lotDate": "2021-05-16", // PK YYYY-MM-DD 16 พฤษภาคม 2564
      "orderNo": "", // SK หมายเลขออเดอร์
      "amount": "", // จำนวน
      "price": "", // ราคา
      "fee": "", // ค่าบริการ

      "phoneNumber": "096254374", // เบอร์โทร
      "name": "",
      "surname": "",
      "note": "", // หมายเหตุ
      "orderType": "ลูกค้า", // ประเภท: ลูกค้า / ตัวแทน

      "buyerName": "",
      "buyerSurname": "",
      "buyerPhoneNumber": "",

      "sellerName": "",
      "sellerSurname": "",
      "sellerPhoneNumber": "",
      "SellerSociallineID": "@lottoonline", // Line

      // Status Order ยังไม่ชำระเงิน / หมดเวลา / รอตรวจสอบการชำระเงิน / ยกเลิก / การสั่งซื้อเสร็จสมบูรณ์
      "statusOrder": "การสั่งซื้อเสร็จสมบูรณ์",

      // Slip
      "locationImg": {
        "filename": "64-19-10-151022.jpg",
        "slip": "https://s3.amazonaws.com/bucketname/foldername/imagename_full.jpg"
      },

      // Lottery
      "lottery": [
        {
          // งวดที่
          "lotDate": "2021-05-16", // YYYY-MM-DD งวด 16 พฤษภาคม 2564
          "lotteryID": "123456_10", // <LotteryNo_LotterySeries>
          "lotDescription": "19", // งวดที่
          "lotterySeries": "10", // ชุด

          // สลาก
          "lotteryNo": "123456",
          "locationImg": {
            "filename": "64-19-10-151022.jpg",
            "mini": "https://s3.amazonaws.com/bucketname/foldername/imagename_mini.jpg",
            "full": "https://s3.amazonaws.com/bucketname/foldername/imagename_full.jpg"
          },

          // Status
          "isSoldOut": false // true:SOLD OUT, false: Available
        }
        // ...
      ],

      "createdAt": "2020-14-03T08:02:10",
      "updatedAt": "2020-14-03T08:02:10"
    }
    // ...
  ]
}
```

## 9. Complain

```json
{
  "listComplain": [
    {
      "userID": "1pfhes9biiur5lidlpi6cdpt5j",

      "titleComplain": "", // ชื่อเรื่องร้องเรียน
      "nameComplainer": "", // ชื่อผู้ร้องเรียน
      "complainType": "", // หัวข้อร้องเรียน
      "phoneNumberComplainer": "096254374", // เบอร์โทรผู้ร้องเรียน
      "emailComplainer": "", // อีเมล์ผู้ร้องเรียน
      "descriptionComplain": "", // รายละเอียด....

      "locationImg": [
        {
          "filename": "64-19-10-151022.jpg",
          "img": "https://s3.amazonaws.com/bucketname/foldername/imagename_full.jpg"
        }
        // ...
      ],

      "status": "OK", // OK / Failed

      "createdAt": "2021-07-16T10:13:32+00:00",
      "updatedAt": "2021-07-16T10:13:32+00:00"
    }
    // ...
  ]
}
```

## 10. Permission (User Role)

```json
{
  // Role Shop
  "userRole": [
    {
      "nameRole": "ลูกค้า" // PK
    },
    {
      "nameRole": "ตัวแทน" // PK
    },
    {
      "nameRole": "admin" // PK
    }
  ]
}
```

<!-- --------------------------------------------------------------------------------------------------------------------------------------------- -->

## 5. Lottery can get reward

```json
{}
```

## 5. Reward

```json
{}
```

## 6. Commission

```json
{}
```
