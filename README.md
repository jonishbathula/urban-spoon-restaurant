# Urban Spoon — MERN Restaurant Website

A fully responsive restaurant website built for the Urban Spoon internship assignment.

## 1. Project scope

This implementation covers all five levels from the supplied assignment:

- **Level 1 — Basic Website Layout**
  - Urban Spoon branding
  - Responsive navigation
  - Hero section
  - Footer with address and opening hours
- **Level 2 — Menu & Information**
  - Menu page
  - Starters, Main Course, Desserts and Beverages
  - Category filtering
  - About section
  - Responsive cards
- **Level 3 — Contact & Timings**
  - Contact page
  - Address, phone, email and opening hours
  - Map/location section
  - Contact form with MongoDB persistence
- **Level 4 — Table Inquiry Backend**
  - Reservation/table inquiry form
  - Express REST API
  - MongoDB/Mongoose persistence
  - Server-side validation
  - Success response and confirmation summary
- **Level 5 — Advanced UI & Practical Enhancements**
  - Menu filtering
  - Admin dashboard for inquiry records
  - Loading/error/empty states
  - Hover effects and responsive UI
  - Mobile navigation
  - API integration with Axios

## 2. Tech stack

### Frontend

- React
- Vite
- React Router
- Axios
- Lucide React
- CSS3 / responsive media queries

### Backend

- Node.js
- Express.js
- MongoDB
- Mongoose
- CORS
- Helmet
- express-rate-limit
- dotenv

React components are intentionally separated into reusable UI pieces, following the component-oriented model described in the React documentation. Vite is used as the build tool for a straightforward React SPA setup. See:
- https://react.dev/learn
- https://react.dev/learn/build-a-react-app-from-scratch

## 3. Architecture

```text
urban-spoon-mern/
├── client/
│   ├── public/
│   │   └── favicon.svg
│   ├── src/
│   │   ├── components/
│   │   │   ├── common/
│   │   │   ├── home/
│   │   │   ├── menu/
│   │   │   └── forms/
│   │   ├── data/
│   │   ├── pages/
│   │   ├── services/
│   │   ├── App.jsx
│   │   ├── main.jsx
│   │   └── styles.css
│   ├── .env.example
│   ├── index.html
│   └── package.json
├── server/
│   ├── src/
│   │   ├── config/
│   │   ├── controllers/
│   │   ├── middleware/
│   │   ├── models/
│   │   ├── routes/
│   │   ├── utils/
│   │   └── server.js
│   ├── .env.example
│   └── package.json
└── README.md
```

## 4. Prerequisites

Install:

- Node.js 20+ recommended
- npm
- MongoDB local server **or** MongoDB Atlas account

Check:

```bash
node -v
npm -v
```

## 5. MongoDB setup

### Option A — MongoDB Atlas

Create a cluster and get the connection string.

Example:

```env
MONGODB_URI=mongodb+srv://USERNAME:PASSWORD@cluster.mongodb.net/urban_spoon?retryWrites=true&w=majority
```

### Option B — Local MongoDB

Use:

```env
MONGODB_URI=mongodb://127.0.0.1:27017/urban_spoon
```

## 6. Backend setup

```bash
cd server
npm install
```

Copy `.env.example` to `.env` and update the MongoDB value.

```env
PORT=5000
MONGODB_URI=mongodb://127.0.0.1:27017/urban_spoon
CLIENT_URL=http://localhost:5173
NODE_ENV=development
```

Start the backend:

```bash
npm run dev
```

Health check:

```text
http://localhost:5000/api/health
```

## 7. Frontend setup

Open a second terminal:

```bash
cd client
npm install
```

Create `.env` from `.env.example`:

```env
VITE_API_URL=http://localhost:5000/api
```

Start:

```bash
npm run dev
```

Open the Vite URL shown in the terminal, normally:

```text
http://localhost:5173
```

## 8. API documentation

### GET /api/health

Checks whether the API is running.

Response:

```json
{
  "success": true,
  "message": "Urban Spoon API is running"
}
```

### POST /api/inquiries

Creates a new table inquiry.

Request:

```json
{
  "name": "Joni",
  "phone": "9876543210",
  "email": "joni@example.com",
  "date": "2026-09-10",
  "time": "19:30",
  "guests": 4,
  "occasion": "Birthday",
  "message": "Window-side table if possible"
}
```

### GET /api/inquiries

Returns all inquiry records. This is used by the demo admin dashboard.

### GET /api/inquiries/:id

Returns one inquiry.

### DELETE /api/inquiries/:id

Deletes one inquiry.

### POST /api/contact

Stores a contact-form message in MongoDB.

Request:

```json
{
  "name": "Joni",
  "email": "joni@example.com",
  "message": "I would like to know about group dining."
}
```

## 9. Routes

Frontend routes:

```text
/          Home
/menu      Menu
/about     About
/contact   Contact
/book      Book a Table
/admin     Admin Dashboard
```

## 10. User flow

```text
Home
  ↓
Explore Menu
  ↓
Select category
  ↓
Book a Table
  ↓
Fill and validate form
  ↓
POST /api/inquiries
  ↓
Express controller
  ↓
Mongoose model
  ↓
MongoDB
  ↓
Success response
  ↓
Confirmation summary
```

## 11. Form validation

Client-side validation checks:

- Required name
- Valid 10-digit Indian-style phone number
- Valid email when provided
- Date cannot be in the past
- Time required
- Guest count between 1 and 20

Server-side validation repeats the important constraints. Never rely only on browser validation.

## 12. Security decisions

The backend includes:

- `helmet` for safer HTTP headers
- `cors` restricted to the configured client origin
- `express-rate-limit` on inquiry submission
- Mongoose schema validation for table inquiries and contact messages
- Environment variables for secrets
- No MongoDB credentials in frontend code

For a real production admin system, add authentication/authorization before exposing inquiry management.

## 13. Images and branding

The UI uses remote food photography URLs from Unsplash and a custom Urban Spoon SVG mark. For a production deployment, keep a record of image licenses/attribution requirements and use official image URLs or locally licensed assets.

If using the Unsplash API rather than direct licensed assets, follow Unsplash's API rules: API responses should use the returned hotlinked image URLs and API photo displays require photographer/Unsplash attribution.
Reference:
- https://help.unsplash.com/en/articles/2511245-unsplash-api-guidelines
- https://help.unsplash.com/en/articles/2511271-guideline-hotlinking-images
- https://help.unsplash.com/en/articles/2511315-guideline-attribution

## 14. Responsive design

Breakpoints:

- Mobile: under 640px
- Tablet: 640px–1023px
- Desktop: 1024px+

The navigation changes into a mobile menu, cards collapse to one column, forms stack vertically, and hero typography scales down.

## 15. Production checklist

Before deployment:

- [ ] Replace demo restaurant contact details
- [ ] Add MongoDB Atlas production URI
- [ ] Set production `CLIENT_URL`
- [ ] Add real admin authentication
- [ ] Move image assets to a controlled/licensed CDN or local storage
- [ ] Add server-side logging
- [ ] Add automated tests
- [ ] Add CSRF strategy if using cookie-based authentication
- [ ] Add pagination to the admin inquiry list
- [ ] Add deployment-specific environment variables
- [ ] Build frontend with `npm run build`
- [ ] Test API against production MongoDB
- [ ] Test on Chrome, Edge, Firefox and mobile widths

## 16. Interview explanation

A concise explanation:

> Urban Spoon is a responsive MERN restaurant application where React handles the component-based UI and routing, Express and Node.js expose REST endpoints, and MongoDB stores table inquiries. I separated the frontend into reusable components and pages, added category-based menu filtering, responsive navigation, client/server validation, API error handling and a basic admin view. The key full-stack flow is the table inquiry form: the React client sends a POST request to Express, the controller validates and persists the data through Mongoose, and the API returns a confirmation object that the UI renders to the user.

## 17. Future enhancements

- JWT-based admin login
- Reservation status workflow
- Email confirmation
- Online payments
- Real Google Maps embed
- CMS for menu management
- Image upload via Cloudinary/S3
- Search and pagination
- Customer reviews
- Order/delivery module
- Automated tests with Vitest/Supertest
