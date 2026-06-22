# 🚀 Bobathon Voting Platform - Deployment Guide

## 📋 Overview

You now have a complete, reusable Bobathon voting platform with:
- **Admin Panel** (`admin.html`) - Password-protected event management
- **Public Voting Page** (`voting.html`) - For participants to vote
- **Multi-event Support** - Create and manage multiple Bobathons
- **One Vote Per Person** - Browser-based voting restriction
- **Real-time Updates** - All votes sync instantly

---

## 🔒 STEP 1: Secure Your Current Firebase Database

### Lock Down Old Database (Do This First!)

1. Go to Firebase Console: https://console.firebase.google.com/
2. Click on **"Bobathon Voting"** project
3. Click **"Realtime Database"** in left sidebar
4. Click **"Rules"** tab at the top
5. Replace everything with:

```json
{
  "rules": {
    ".read": false,
    ".write": false
  }
}
```

6. Click **"Publish"**
7. ✅ Your old database is now locked and preserved

---

## 🔧 STEP 2: Update Firebase Security Rules

### Apply New Security Rules

1. Still in Firebase Console → Realtime Database → Rules tab
2. Replace the rules with this (copy everything):

```json
{
  "rules": {
    "events": {
      "$eventId": {
        ".read": true,
        "projects": {
          ".write": true,
          "$projectIndex": {
            "title": {
              ".write": true
            },
            "description": {
              ".write": true
            },
            "emoji": {
              ".write": true
            },
            "votes": {
              ".write": true
            }
          }
        }
      }
    },
    "settings": {
      ".read": true,
      "activeEventId": {
        ".write": "auth != null"
      }
    },
    "admin": {
      ".read": "auth != null",
      ".write": "auth != null"
    }
  }
}
```

3. Click **"Publish"**

### What These Rules Do:

✅ **Anyone can:**
- Read all event data
- Edit project titles, descriptions, and emojis
- Vote on projects

❌ **Only authenticated admins can:**
- Change which event is active
- Access admin settings

🔒 **Security Features:**
- Prevents deletion of events
- Prevents changing event settings
- Allows public voting and project editing

---

## 📁 STEP 3: Upload Files to GitHub

### Files to Upload:

1. **admin.html** - Admin panel (NEW)
2. **voting.html** - Public voting page (NEW)
3. **index.html** - Keep your old one or replace with voting.html

### Option A: Replace index.html (Recommended)

```bash
# In your project directory
cd "/Users/nina.nash/Desktop/Bob Projects/Genworth Pre Bobathon Survey"

# Backup old index.html
mv index.html index-old.html

# Use voting.html as the new index.html
cp voting.html index.html

# Add all files to git
git add admin.html voting.html index.html

# Commit
git commit -m "Add reusable Bobathon voting platform with admin panel"

# Push to GitHub
git push
```

### Option B: Keep Both Files

```bash
# Just add the new files
git add admin.html voting.html

# Commit
git commit -m "Add admin panel and new voting page"

# Push
git push
```

---

## 🌐 STEP 4: Access Your Platform

### URLs (replace with your actual GitHub Pages URL):

**Admin Panel:**
```
https://[your-username].github.io/[repo-name]/admin.html
```

**Public Voting:**
```
https://[your-username].github.io/[repo-name]/voting.html
```
OR (if you replaced index.html):
```
https://[your-username].github.io/[repo-name]/
```

---

## 🔑 STEP 5: First-Time Setup

### 1. Change Admin Password (IMPORTANT!)

1. Open `admin.html` in your browser
2. Login with default password: **`bobathon2026`**
3. Go to **Settings** tab
4. Enter a new secure password (at least 6 characters)
5. Click "Change Password"
6. ✅ Your admin panel is now secure!

### 2. Create Your First Event

1. In admin panel, go to **Create Event** tab
2. Fill in:
   - **Event Title**: e.g., "Fall 2026 Bobathon"
   - **Event Date**: Select date
   - **Number of Projects**: Choose 4-8
   - **Activate Now**: Check this box
3. Click "Create Event"
4. ✅ Your event is now live!

### 3. Share Voting Link

Share the public voting URL with participants:
```
https://[your-username].github.io/[repo-name]/voting.html
```

---

## 📖 STEP 6: How to Use for Each Bobathon

### Before the Event:

1. **Admin creates event:**
   - Login to admin panel
   - Create new event with title and date
   - Choose number of projects
   - Activate the event

2. **Participants add project info:**
   - Open voting page
   - Click "Edit Your Project Info"
   - Click on project title and description to edit
   - Changes save automatically

### During Voting:

1. **Participants vote:**
   - Click vote button on their favorite project
   - Can only vote once (browser-based)
   - Votes update in real-time for everyone

2. **View results:**
   - Anyone can click "Show Results"
   - See winner and rankings
   - Results update live

### After the Event:

1. **Admin can:**
   - View results in admin panel
   - Create next event
   - Previous event stays in history

---

## 🎯 Admin Panel Features

### Dashboard Tab
- View total events
- See total votes across all events
- Check active event status

### Create Event Tab
- Create new Bobathon event
- Set title, date, and number of projects
- Activate immediately or later

### Manage Events Tab
- View all events (past and present)
- Activate/deactivate events
- Reset votes for an event
- Delete events
- View event details

### Settings Tab
- Change admin password
- Delete all data (danger zone)

---

## 🔐 Security Notes

### What's Protected:
✅ Admin panel requires password
✅ Only one vote per browser
✅ Event settings protected
✅ Can't delete events from public page

### What's Public:
⚠️ Anyone can edit project titles/descriptions (by design)
⚠️ Anyone can see vote counts
⚠️ Voting is browser-based (can be bypassed with incognito)

### For Internal Company Events:
This security level is appropriate because:
- Participants are trusted employees
- Browser-based voting is "good enough"
- Easy for everyone to use
- No complex login required

---

## 🆘 Troubleshooting

### "No active event" message
- Login to admin panel
- Go to Manage Events
- Click "Activate" on the event you want

### Changes not showing up
- Hard refresh: `Cmd + Shift + R` (Mac) or `Ctrl + Shift + R` (Windows)
- Or open in incognito mode

### Can't login to admin
- Default password is: `bobathon2026`
- If you changed it and forgot, you'll need to reset in Firebase Console

### Votes not syncing
- Check connection status at top of page
- Should say "✅ Connected"
- If not, check Firebase rules are applied correctly

---

## 📊 Best Practices

### Before Each Bobathon:
1. Create event 1-2 days before
2. Share link with teams to add project info
3. Remind teams to fill in their project details
4. Activate voting when ready

### During Bobathon:
1. Monitor admin dashboard
2. Can reset votes if needed
3. Show results when voting closes

### After Bobathon:
1. Keep event in history
2. Create new event for next time
3. Old events stay accessible

---

## 🎉 You're All Set!

Your reusable Bobathon voting platform is ready to use for all future events!

### Quick Start Checklist:
- [ ] Lock down old Firebase database
- [ ] Apply new Firebase security rules
- [ ] Upload files to GitHub
- [ ] Change admin password
- [ ] Create first event
- [ ] Test voting with a friend
- [ ] Share link with team

### Need Help?
- Check Firebase Console for errors
- View browser console (F12) for debugging
- Make sure all files are uploaded to GitHub
- Verify GitHub Pages is enabled

---

**Happy Bobathon! 🏆**