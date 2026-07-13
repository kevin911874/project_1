# 🛍️ Local Store E-Commerce Platform

A full-featured e-commerce platform for a local store, enabling customers to browse, search, and purchase products online with complete order management, coupon system, and customer support.

---

## 🚀 Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React.js + Vite |
| Backend | Node.js + Express.js |
| Database | MongoDB Atlas |
| Authentication | JWT (Access + Refresh Tokens) + bcrypt |
| Image Storage | Multer + Cloudinary |
| Payment | Razorpay + Cash on Delivery |
| Email | Nodemailer |
| PDF | PDFKit |

---

## 📁 Project Structure

```
local-store-ecommerce/
├── server/
│   ├── config/
│   │   ├── db.js                    # MongoDB Atlas connection
│   │   └── cloudinary.js            # Cloudinary setup + helpers
│   ├── models/
│   │   ├── User.js                  # Users with addresses & wishlist
│   │   ├── Product.js               # Products with images & specs
│   │   ├── Category.js              # Hierarchical categories
│   │   ├── Cart.js                  # Shopping cart
│   │   ├── Order.js                 # Orders with status history
│   │   ├── Review.js                # Product reviews with approval
│   │   ├── Coupon.js                # Discount coupons
│   │   └── SupportTicket.js         # Customer support tickets
│   ├── middleware/
│   │   ├── authMiddleware.js         # JWT protect & optionalAuth
│   │   ├── roleMiddleware.js         # RBAC (isAdmin, authorizeRoles)
│   │   ├── uploadMiddleware.js       # Multer + Cloudinary storage
│   │   └── errorHandler.js          # Global error handler + asyncHandler
│   ├── controllers/                  # All business logic
│   ├── routes/                       # All Express routers
│   ├── utils/
│   │   ├── apiFeatures.js            # Reusable search/filter/sort/paginate
│   │   ├── sendEmail.js              # Nodemailer + HTML email templates
│   │   └── generateInvoice.js        # PDFKit invoice generator
│   ├── server.js                     # Main entry point
│   ├── .env                          # Environment variables (fill in)
│   └── package.json
└── .gitignore
```

---

## ⚡ Quick Start

### 1. Fill in environment variables
```bash
# Edit server/.env with your actual values
```

### 2. Install & run
```bash
cd server
npm install
npm run dev
```
Server → **http://localhost:5000**

---

## 📡 Full API Reference

### Auth `/api/auth`
| POST `/register` | POST `/login` | POST `/logout` | POST `/refresh-token` |
| POST `/forgot-password` | PUT `/reset-password/:token` | GET `/me` |
| PUT `/update-profile` | PUT `/change-password` |
| POST `/add-address` | PUT `/addresses/:id` | DELETE `/addresses/:id` |
| POST `/wishlist/:productId` |

### Products `/api/products`
| GET `/` *(search, filter, sort, paginate)* | GET `/featured` | GET `/:slug` |
| GET `/low-stock` *[Admin]* | GET `/admin/all` *[Admin]* |
| POST `/` *[Admin+images]* | PUT `/:id` *[Admin]* | DELETE `/:id` *[Admin]* |
| PUT `/:id/toggle-featured` *[Admin]* |

### Categories `/api/categories`
| GET `/` | GET `/:slug` | POST `/` *[Admin]* | PUT `/:id` *[Admin]* | DELETE `/:id` *[Admin]* |

### Cart `/api/cart`
| GET `/` | POST `/add` | PUT `/update` | DELETE `/remove/:productId` |
| DELETE `/clear` | POST `/apply-coupon` | DELETE `/remove-coupon` |

### Orders `/api/orders`
| POST `/place` | GET `/my-orders` | GET `/:id` | GET `/:id/invoice` |
| POST `/razorpay/create` | POST `/razorpay/verify` |
| PUT `/:id/cancel` | PUT `/:id/return` |
| GET `/admin/all` *[Admin]* | PUT `/:id/status` *[Admin]* |

### Reviews `/api/reviews`
| GET `/product/:productId` | POST `/:productId` | PUT `/:id` | DELETE `/:id` | PUT `/:id/like` |
| GET `/pending` *[Admin]* | PUT `/:id/approve` *[Admin]* | PUT `/:id/reply` *[Admin]* |

### Coupons `/api/coupons`
| POST `/validate` | GET `/` *[Admin]* | POST `/` *[Admin]* | PUT `/:id` *[Admin]* |
| DELETE `/:id` *[Admin]* | PUT `/:id/toggle` *[Admin]* |

### Support `/api/support`
| GET `/my-tickets` | POST `/create` | GET `/:id` | POST `/:id/reply` | PUT `/:id/close` |
| GET `/admin/all` *[Admin]* | PUT `/:id/status` *[Admin]* |

### Admin `/api/admin`
| GET `/dashboard` | GET `/revenue/chart` | GET `/users` | GET `/users/:id` |
| PUT `/users/:id/toggle-block` | PUT `/users/:id/role` |

---

## 🔐 Authentication

```
POST /register or /login
→ Returns: { accessToken } + sets httpOnly refreshToken cookie

Protected routes:
Authorization: Bearer <accessToken>

Token refresh:
POST /refresh-token (reads cookie automatically)
→ Returns: { accessToken }
```

---

## 🛡️ Security

- **Helmet** — Secure HTTP headers
- **CORS** — Whitelist frontend origin
- **Rate Limiting** — 100/15min global, 10/15min on auth
- **Mongo Sanitize** — NoSQL injection prevention
- **XSS Clean** — HTML sanitization
- **HPP** — HTTP parameter pollution prevention
- **bcrypt salt=12** — Password hashing
- **HttpOnly Cookie** — Refresh token unreachable from JS

---

## 📧 Email Templates

| Template | Trigger |
|----------|---------|
| `welcome` | User registration |
| `passwordReset` | Forgot password |
| `orderConfirmation` | Order placed |
| `orderStatusUpdate` | Status change |
| `supportReply` | Admin replies to ticket |
