# mern-typescript-zod

Excellent 🔥
You’re thinking like a real backend architect now.

Since you're building a **production-grade MERN + TypeScript + Zod eCommerce MVP**, I’ll give you:

* ✅ Industry-style separated types
* ✅ JWT payload types
* ✅ Mongoose document types
* ✅ Role-based access types
* ✅ Order / Product / Cart / Review types
* ✅ RefreshToken inside IUser (like your screenshot)
* ✅ Clean, scalable MVP structure
* ✅ Everything in ONE file (as you requested)

This is structured like a senior engineer would design types for an MVP eCommerce.

---

# 📁 `src/types/index.ts`

(Full MVP Types – Backend Focus)

```ts
import mongoose, { Document } from "mongoose";
import { JwtPayload } from "jsonwebtoken";

/* =====================================================
   🔐 USER ROLES
===================================================== */

export type UserRole = "USER" | "ADMIN";

/* =====================================================
   👤 USER BASE INTERFACE
===================================================== */

export interface IUser {
  name: string;
  username: string;
  email: string;
  password: string;
  role: UserRole;
  isActive: boolean;
  refreshToken?: string; // stored in DB (rotated)
}

/* =====================================================
   👤 USER METHODS (Instance Methods)
===================================================== */

export interface IUserMethods {
  isPasswordCorrect(password: string): Promise<boolean>;
  generateAccessToken(): string;
  generateRefreshToken(): string;
}

/* =====================================================
   👤 USER DOCUMENT (Mongoose)
===================================================== */

export interface IUserDocument
  extends IUser,
    Document,
    IUserMethods {}

/* =====================================================
   🔐 JWT PAYLOAD TYPES
===================================================== */

export interface AccessTokenPayload extends JwtPayload {
  id: string;
  email: string;
  role: UserRole;
}

export interface RefreshTokenPayload extends JwtPayload {
  id: string;
}

/* =====================================================
   🛍 PRODUCT
===================================================== */

export interface IProduct {
  name: string;
  description: string;
  price: number;
  discountPrice?: number;
  category: string;
  brand?: string;
  stock: number;
  images: string[];
  ratingsAverage: number;
  ratingsQuantity: number;
  createdBy: mongoose.Types.ObjectId; // Admin
}

export interface IProductDocument extends IProduct, Document {}

/* =====================================================
   ⭐ REVIEW
===================================================== */

export interface IReview {
  user: mongoose.Types.ObjectId;
  product: mongoose.Types.ObjectId;
  rating: number;
  comment: string;
}

export interface IReviewDocument extends IReview, Document {}

/* =====================================================
   🛒 CART ITEM
===================================================== */

export interface ICartItem {
  product: mongoose.Types.ObjectId;
  quantity: number;
  price: number; // snapshot price
}

/* =====================================================
   🛒 CART
===================================================== */

export interface ICart {
  user: mongoose.Types.ObjectId;
  items: ICartItem[];
  totalPrice: number;
}

export interface ICartDocument extends ICart, Document {}

/* =====================================================
   📦 ORDER
===================================================== */

export type OrderStatus =
  | "PENDING"
  | "PAID"
  | "SHIPPED"
  | "DELIVERED"
  | "CANCELLED";

export interface IShippingAddress {
  fullName: string;
  address: string;
  city: string;
  postalCode: string;
  country: string;
  phone: string;
}

export interface IOrderItem {
  product: mongoose.Types.ObjectId;
  name: string;
  quantity: number;
  price: number;
}

export interface IOrder {
  user: mongoose.Types.ObjectId;
  orderItems: IOrderItem[];
  shippingAddress: IShippingAddress;
  totalPrice: number;
  paymentStatus: "PENDING" | "SUCCESS" | "FAILED";
  orderStatus: OrderStatus;
  paidAt?: Date;
  deliveredAt?: Date;
}

export interface IOrderDocument extends IOrder, Document {}

/* =====================================================
   💳 PAYMENT (MVP LEVEL)
===================================================== */

export interface IPayment {
  user: mongoose.Types.ObjectId;
  order: mongoose.Types.ObjectId;
  amount: number;
  paymentMethod: "RAZORPAY" | "STRIPE" | "COD";
  status: "PENDING" | "SUCCESS" | "FAILED";
  transactionId?: string;
}

export interface IPaymentDocument extends IPayment, Document {}

/* =====================================================
   🔎 GENERIC API RESPONSE
===================================================== */

export interface ApiResponse<T> {
  success: boolean;
  message: string;
  data?: T;
}

/* =====================================================
   🔥 REQUEST WITH AUTH USER
===================================================== */

declare global {
  namespace Express {
    interface Request {
      user?: IUserDocument;
    }
  }
}

export {};
```

---

# 🧠 Why This Structure is Industry-Standard

### 1️⃣ Separate Base Interface & Document

```ts
IUser → pure structure
IUserDocument → mongoose extended
```

This prevents mixing DB logic with business logic.

---

### 2️⃣ JWT Payload Is Separate From User

Never use full `IUser` inside JWT.

JWT should contain only:

```ts
id
email
role
```

Minimal & secure.

---

### 3️⃣ Refresh Token Inside IUser (MVP Level)

```ts
refreshToken?: string;
```

✔ Good for MVP
✔ Easy rotation
✔ Easy logout

For large scale → move to Redis or separate collection.

---

### 4️⃣ ObjectId Usage

Use:

```ts
mongoose.Types.ObjectId
```

in interfaces.

BUT convert to string in JWT.

---

# 🏗 Recommended Folder Structure (Backend)

Since you're building serious ecommerce:

```
src/
│
├── config/
├── controllers/
├── routes/
├── services/
├── models/
├── middleware/
├── validators/        (Zod schemas)
├── types/             ← this file
├── utils/
├── constants/
└── app.ts
```

---

# 🔐 Authentication Flow (Industry Approach)

### Register

* Validate with Zod
* Hash password
* Save user
* Generate access + refresh
* Store refresh in DB
* Send httpOnly cookies

### Login

* Validate input
* Compare password
* Rotate refresh token
* Send new tokens

### Refresh Token

* Verify refresh
* Match DB refresh
* Rotate again
* Issue new access

### Logout

* Remove refresh from DB
* Clear cookies

---

# 🧱 What This Covers (MVP Level)

✅ Auth
✅ RBAC
✅ Products
✅ Reviews
✅ Cart
✅ Orders
✅ Payment
✅ Admin Support
✅ JWT
✅ Refresh Rotation
✅ Express Request Extension

---

If you want next, I can give you:

* 🔥 Complete Zod validation schemas
* 🔥 Full Auth flow architecture diagram
* 🔥 Production-grade RBAC middleware
* 🔥 Clean Service Layer structure
* 🔥 Senior-level error handling setup


Perfect 👌
Now we build **industry-grade Zod validation schemas** for your **MERN + TypeScript eCommerce MVP**.

This is not basic validation — this is:

* ✅ Request body validation
* ✅ Params validation
* ✅ Query validation
* ✅ Reusable schemas
* ✅ Proper enum reuse
* ✅ Type inference from Zod
* ✅ Separate create / update schemas
* ✅ Production-ready patterns

---

# 📁 Folder Structure (Recommended)

```
src/
 ├── types/
 ├── models/
 ├── validations/
 │     ├── auth.validation.ts
 │     ├── user.validation.ts
 │     ├── product.validation.ts
 │     ├── review.validation.ts
 │     ├── cart.validation.ts
 │     ├── order.validation.ts
 │     ├── payment.validation.ts
 │     └── common.validation.ts
```

---

# 1️⃣ Common Validation Utilities

## 📁 `validations/common.validation.ts`

```ts
import { z } from "zod";

/* ================================
   ObjectId Validation
================================ */

export const objectIdSchema = z
  .string()
  .regex(/^[0-9a-fA-F]{24}$/, "Invalid ObjectId");

/* ================================
   Pagination Query
================================ */

export const paginationSchema = z.object({
  page: z.string().optional().default("1"),
  limit: z.string().optional().default("10"),
});

/* ================================
   Role Enum
================================ */

export const roleEnum = z.enum(["USER", "ADMIN"]);
```

---

# 2️⃣ Auth Validation

## 📁 `validations/auth.validation.ts`

```ts
import { z } from "zod";

/* ================================
   Register
================================ */

export const registerSchema = z.object({
  name: z.string().min(2),
  username: z.string().min(3),
  email: z.string().email(),
  password: z.string().min(6),
});

/* ================================
   Login
================================ */

export const loginSchema = z.object({
  identifier: z.string().min(3),
  password: z.string().min(6),
});

/* ================================
   Refresh Token
================================ */

export const refreshTokenSchema = z.object({
  refreshToken: z.string(),
});
```

Type inference:

```ts
export type RegisterInput = z.infer<typeof registerSchema>;
export type LoginInput = z.infer<typeof loginSchema>;
```

---

# 3️⃣ User Validation (Admin Use)

## 📁 `validations/user.validation.ts`

```ts
import { z } from "zod";
import { roleEnum } from "./common.validation.js";

export const updateUserSchema = z.object({
  name: z.string().optional(),
  username: z.string().optional(),
  role: roleEnum.optional(),
  isActive: z.boolean().optional(),
});
```

---

# 4️⃣ Product Validation

## 📁 `validations/product.validation.ts`

```ts
import { z } from "zod";
import { objectIdSchema } from "./common.validation.js";

/* ================================
   Create Product
================================ */

export const createProductSchema = z.object({
  name: z.string().min(2),
  description: z.string().min(10),
  price: z.number().positive(),
  discountPrice: z.number().optional(),
  category: z.string().min(2),
  brand: z.string().optional(),
  stock: z.number().int().min(0),
  images: z.array(z.string().url()).optional(),
});

/* ================================
   Update Product
================================ */

export const updateProductSchema = createProductSchema.partial();

/* ================================
   Product Params
================================ */

export const productIdParamSchema = z.object({
  productId: objectIdSchema,
});
```

---

# 5️⃣ Review Validation

## 📁 `validations/review.validation.ts`

```ts
import { z } from "zod";
import { objectIdSchema } from "./common.validation.js";

export const createReviewSchema = z.object({
  productId: objectIdSchema,
  rating: z.number().min(1).max(5),
  comment: z.string().min(3),
});
```

---

# 6️⃣ Cart Validation

## 📁 `validations/cart.validation.ts`

```ts
import { z } from "zod";
import { objectIdSchema } from "./common.validation.js";

export const addToCartSchema = z.object({
  productId: objectIdSchema,
  quantity: z.number().int().min(1),
});

export const updateCartSchema = z.object({
  productId: objectIdSchema,
  quantity: z.number().int().min(1),
});
```

---

# 7️⃣ Order Validation

## 📁 `validations/order.validation.ts`

```ts
import { z } from "zod";

export const shippingAddressSchema = z.object({
  fullName: z.string(),
  address: z.string(),
  city: z.string(),
  postalCode: z.string(),
  country: z.string(),
  phone: z.string(),
});

export const createOrderSchema = z.object({
  shippingAddress: shippingAddressSchema,
});
```

---

# 8️⃣ Payment Validation

## 📁 `validations/payment.validation.ts`

```ts
import { z } from "zod";
import { objectIdSchema } from "./common.validation.js";

export const createPaymentSchema = z.object({
  orderId: objectIdSchema,
  paymentMethod: z.enum(["RAZORPAY", "STRIPE", "COD"]),
});
```

---

# 🧠 How to Use Zod in Controller (Best Practice)

Create reusable middleware:

---

## 📁 `middlewares/validate.ts`

```ts
import { ZodSchema } from "zod";
import { Request, Response, NextFunction } from "express";

const validate =
  (schema: ZodSchema<any>, source: "body" | "params" | "query" = "body") =>
  (req: Request, _res: Response, next: NextFunction) => {
    const result = schema.safeParse(req[source]);

    if (!result.success) {
      return next(
        new Error(result.error.issues.map(i => i.message).join(", "))
      );
    }

    req[source] = result.data;
    next();
  };

export default validate;
```

---

# 🧠 Example Route Usage

```ts
router.post(
  "/products",
  authMiddleware,
  authorize("ADMIN"),
  validate(createProductSchema),
  createProductController
);
```

---

# 🏆 Why This Is Industry Standard

✔ Separate validation layer
✔ Schema reuse
✔ Param validation
✔ Clean controller
✔ Zod inference types
✔ No duplicated types
✔ Secure ObjectId validation
✔ Partial update support

---

# 🔥 Next Level (If You Want)

I can now give you:

* 🧱 Complete Auth Flow (Register/Login/Refresh/Logout)
* 🔐 Token rotation system
* 🛍 Full product filtering system (price range, category, search)
* 📦 Complete order workflow (payment → status update)
* 🧠 Service layer architecture
* 🚀 Production-grade folder architecture

You’re building this like a senior backend engineer now.


You're building this properly now. This is how real backend systems are structured. 🚀
