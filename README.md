# CelebWeave MVP Prototype

A lightweight, single-file event management platform for celebrations. Built with Vanilla JS, Tailwind CSS, and localStorage.

**Live Demo**: Deploy to Vercel (see instructions below)

---

## 🎯 Features

- **Event Management** — Create events, set dates, venues, dress codes
- **Guest Management** — Add guests via form, manage lists
- **Smart RSVP** — Public RSVP page, guests accept/decline
- **QR Code Check-in** — Generate unique QR codes per guest
- **Check-in Simulator** — Simulate venue check-in flow
- **Persistent Storage** — Data saved to localStorage

---

## 🚀 Quick Start

### Local Testing
1. Download `index.html`
2. Open in a browser
3. Start creating events!

### Deploy to Vercel (Recommended)

1. **Create GitHub Repo**
   ```bash
   mkdir celebweave-mvp
   cd celebweave-mvp
   git init
   git add index.html README.md
   git commit -m "Initial CelebWeave MVP"
   git remote add origin https://github.com/YOUR_USERNAME/celebweave-mvp.git
   git push -u origin main
   ```

2. **Deploy to Vercel**
   - Go to https://vercel.com
   - Click "Import Project"
   - Select your GitHub repo
   - Click Deploy
   - Done! Live in ~30 seconds

---

## 📱 User Flow

### For Event Organizers
1. Login with email
2. Create Event (name, date, venue, dress code, max guests)
3. Add Guests (manually or bulk)
4. Share RSVP Link (WhatsApp, Email, or copy)
5. Monitor Responses (dashboard shows accept/decline count)
6. Check-in Guests at Venue (scan QR codes)

### For Guests
1. Receive invite link (WhatsApp/Email)
2. Click link
3. Enter name & email
4. Accept or Decline
5. Receive QR code
6. Show QR at venue check-in

---

## 🛠️ Tech Stack

| Layer | Tech |
|-------|------|
| **Frontend** | HTML5 + Vanilla JS |
| **Styling** | Tailwind CSS (CDN) |
| **QR Codes** | QRCode.js library |
| **Storage** | Browser localStorage |
| **Deployment** | Vercel (static hosting) |

---

## 📝 File Structure

```
celebweave-mvp/
├── index.html          # Single-file app (everything bundled)
├── README.md           # This file
└── .gitignore          # Git ignore file
```

---

## 🎨 UI/UX Highlights

- **Gradient Hero** — Modern purple-to-pink design
- **Responsive Layout** — Works on mobile, tablet, desktop
- **Card-Based UI** — Clean, scannable interface
- **Color-Coded Status** — Green (accepted), Red (declined), Yellow (pending)
- **Form Validation** — Basic input checking

---

## 💾 Data Storage

All data is stored in **browser localStorage** (not a database).

**Stored Data:**
- User login info
- Events
- Guest lists
- RSVP responses
- Check-in records

**Clearing Data:**
```javascript
localStorage.clear()  // Clears everything
```

---

## 🔜 Next Steps (Roadmap)

### Phase 1 (MVP - This Prototype)
- ✅ Event creation & management
- ✅ Guest invitations
- ✅ RSVP flow
- ✅ QR check-in
- ✅ Persistent storage

### Phase 2 (Backend Integration)
- ⏳ Real database (Firebase or PostgreSQL)
- ⏳ User authentication (proper auth)
- ⏳ Email notifications
- ⏳ SMS reminders

### Phase 3 (Platform Features)
- ⏳ Vendor marketplace (photographers, caterers, etc.)
- ⏳ Aso Ebi selection & checkout
- ⏳ Payment processing
- ⏳ Analytics dashboard
- ⏳ Mobile app

### Phase 4 (Scale)
- ⏳ Team collaboration
- ⏳ Event branding
- ⏳ Public APIs
- ⏳ Enterprise features

---

## 🐛 Known Limitations

- **No Real Database** — Uses localStorage (data lost if cache cleared)
- **No Authentication** — Demo login only
- **No Real Email/SMS** — Invites are links only
- **QR Scanner Simulated** — Click to simulate; real camera needed for production
- **No Payment Processing** — No Stripe/PayPal integration yet

---

## 🚀 Production Roadmap

To convert this prototype to production:

1. **Backend** — Replace localStorage with API (Node.js + Express or Django)
2. **Database** — PostgreSQL + Redis for caching
3. **Auth** — JWT tokens + OAuth (Google, Facebook)
4. **Email** — SendGrid or AWS SES
5. **QR Scanner** — Real camera integration (HTML5 getUserMedia)
6. **Payments** — Stripe API
7. **Hosting** — AWS, GCP, or Heroku (backend), Vercel (frontend)
8. **Automation** — n8n workflows for emails, reminders, etc.

---

## 📞 Support

For issues or feature requests, open a GitHub issue.

---

## 📄 License

MIT License — Free to use and modify for personal/commercial projects.

---

**Built for CelebWeave — Celebrate Without Stress**
