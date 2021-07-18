# Lottery Online Document

# About

....

# Database

## 1. Registration and Login

## 2. Bank

```typescript
{
  "bank": [
    {
      "bankCompanyAbbreviation": "SCB", // PK
      "bankCompany": "ไทยพาณิชย์ (Siam Commercial Bank)" // SK
    },
    {
      "bankCompanyAbbreviation": "KBANK",
      "bankCompany": "กสิกรไทย (Kasikornbank)"
    }
    // ...
  ]
}
```

## 2. Lot of Lottery

```typescript
{
  "lot": [
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

## 3. Shop

```typescript
{
  "shop": [
    {
      // User
      "phoneNumber": "096254374", // PK เบอร์โทร

      // Shop
      "shopID": "2xji1s", // SK รหัสตัวแทน
      "name": "ภราดร",
      "surname": "ศรีชาพันธุ์",
      "email": "donut@gmail.com",
      "idCard": "1100900984382", // เลขบัตรประชาชน
      "idCardImg": "https://s3.amazonaws.com/bucketname/foldername/imagename_idcard.jpg", // เลขบัตรประชาชน
      "roleShop": "ยี่ปั้ว", // ชื่อธนาคาร Relation with `Role of seller Table`
      "upline": "<phoneNumber>", // Upline's Hierarchy Relation with `User Table`

      // Shop Bank
      "bankCompanyAbbreviation": "SCB", // ชื่อธนาคาร Relation with `Bank Table`
      "bankNumber": "72093208283", // เลขที่บัญชี

      // Social Media
      "socialFacebookID": "Facebook", // Facebook
      "sociallineID": "@lottoonline", // Line

      // Quota
      "quotaType": "", // ประเภทโควต้า - โควต้าขายขาด, โควต้าลอย
      "creditType": "", // รูปแบบเครดิต - โอนขาด, ไม่จำกัด
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

      // Status
      "isOpen": true, // true: Open, false: Close
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

## 4. Quota of shop (Transaction)

```typescript
{
  "transactionQuota": [
    {
      "lotDate": "2021-05-16", // PK YYYY-MM-DD งวด 16 พฤษภาคม 2564

      // Sender
      "shopSenderID": "2xji1s", // SK รหัสตัวแทน
      "nameSender": "ภราดร",
      "surnameSender": "ศรีชาพันธุ์",

      // Receiver
      "shopReceiverID": "2xji1s",
      "nameReceiver": "ภราดร",
      "surnameReceiver": "ศรีชาพันธุ์",
      "quota": 2000.0,

      "createdAt": "2021-07-16T10:13:32+00:00", // Datetime
      "updatedAt": "2021-07-16T10:13:32+00:00"
    }
    // ...
  ]
}
```

## 4. Lottery

<!-- Import lottery: How do you tracking ? -->

```typescript
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
      "orderNo": "11312624917052", // Relative Order of lottery (Transaction) Table

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

## 4. Cart

```typescript
{
  "cart": [
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
      "SellerSociallineID": "@lottoonline", // Line

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

## 5. Order of lottery (Transaction)

```typescript
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

## 2. Users

```typescript
{
  "phoneNumber": "0962963233", // PK
  "bankCompanyAbbreviation": "KBANK",
  "bankNumber": "72093208283",
  "name": "ภราดร",
  "surname": "ศรีชาพันธุ์",
  "roleType": "ลูกค้า", // ลูกค้า / ตัวแทน / admin
  "roleClass": "ลูกค้า", // ลูกค้า / ยี่ปั้ว / ซาปั้ว / สี่ปั้ว / โหงวปั้ว / admin

  "shopFriend": [
    {
      "phoneNumber": "096254374",
      "shopID": "2xji1s"
    },
    {
      "phoneNumber": "096254374",
      "shopID": "2xji1s"
    }
    // ...
  ],

  "permissionAdmin": {
    "delegator": false, // ตัวแทน
    "importLottery": false, // สลาก
    "financeReport": false, // การเงิน
    "contentMarketing": false, // สื่อสาร
    "report": false, // รายงาน
    "createAdmin": false // สร้างผู้ดูแล
  },

  "createdAt": "2021-07-16T10:13:32+00:00",
  "updatedAt": "2021-07-16T10:13:32+00:00"
}
```

## 4. Complain

```typescript
{
  "listComplain": [
    {
      "phoneNumber": "096254374",

      "titleComplain": "096254374", // ชื่อเรื่องร้องเรียน
      "nameComplainer": "", // ชื่อผู้ร้องเรียน
      "complainType": "096254374", // หัวข้อร้องเรียน
      "phoneNumberComplainer": "096254374", // เบอร์โทรผู้ร้องเรียน
      "emailComplainer": "096254374", // อีเมล์ผู้ร้องเรียน
      "descriptionComplain": "", // รายละเอียด....

      "locationImg": [
        {
          "filename": "64-19-10-151022.jpg",
          "img": "https://s3.amazonaws.com/bucketname/foldername/imagename_full.jpg"
        }
        // ...
      ],

      "createdAt": "2021-07-16T10:13:32+00:00",
      "updatedAt": "2021-07-16T10:13:32+00:00"
    }
    // ...
  ]
}
```

## 3. Permission (User Role)

```typescript
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

# Back Office

## 1. Admin

```typescript
{
  "userID": "user01",
  "username": "0962963233",
  "password": "KBANK",
  "name": "ภราดร",
  "surname": "ศรีชาพันธุ์"
}
```

## 5. Lottery can get reward

```typescript
{
}
```

## 5. Reward

```typescript
{
}
```

## 6. Commission

```typescript
{
}
```
