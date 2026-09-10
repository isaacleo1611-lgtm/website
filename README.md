# website
Leobrain Technology — Web App
A single-file web app for Leobrain Technology: a public service menu/home page with a request form, an admin dashboard, and user accounts with live chat.
File: leobrain_app.html — open it in a browser, or load it as a Claude artifact (it uses Claude’s window.storage API for data, so it needs to run inside the Claude artifact environment to save/load anything).
What’s inside
1. Home page (public)
	•	Full rebranded service menu: flyer, logo, branding, video, and tech-support packages.
	•	“Request a Quote / Send Feedback” form that:
	•	Submits to your Formspree endpoint: https://formspree.io/f/xppzqlow
	•	Also saves a copy into shared storage so it shows up in the Admin Dashboard’s Requests & Feedback tab.
2. Admin Login
	•	Click Admin Login in the top nav.
	•	Demo credentials (hardcoded in the <script>, near the top):
const ADMIN_USER = 'admin';
const ADMIN_PASS = '********';
3. Admin Dashboard
Three tabs:
	•	Requests & Feedback — every message submitted through the home page form.
	•	Live Chats — a list of everyone who has started a chat; click a name to open the thread and reply.
	•	Registered Users — everyone who has signed up through User Register.
4. User Login / Register
	•	Register — username, email, password. Stored in shared storage (plain text — see Limitations).
	•	Login — matches username + password against stored accounts.
5. User Dashboard
	•	“Welcome, {username}” header.
	•	Start Live Chat with Admin button — opens a chat box. Messages are saved to shared storage and picked up by the admin panel (and vice versa) via polling every ~3–4 seconds.
How data is stored
Everything persists via Claude’s artifact storage API (window.storage), scoped as shared so admin and users see the same data:
|Key                       |Contents                                                          |
|--------------------------|------------------------------------------------------------------|
|`leobrain-feedback`       |Array of form submissions (name, email, message, timestamp)       |
|`leobrain-users`          |Array of registered users (username, email, password, joined date)|
|`leobrain-chat-index`     |List of usernames who have started a chat                         |
|`leobrain-chat-<username>`|That user’s message thread with admin                             |
Known limitations (read before using for real customers)
	•	No real backend. This is client-side only. Anyone who opens the file and reads the source can see the admin password and how data is stored.
	•	Passwords are stored in plain text, not hashed. Fine for a demo/internal tool, not safe for real customer accounts.
	•	No true real-time chat. Both sides poll shared storage every few seconds rather than using a live socket connection — there will be a short delay between sending and seeing a reply.
	•	Login doesn’t persist across page reloads. Browser storage (localStorage/sessionStorage) isn’t available in this environment, so refreshing the page logs everyone out. This is expected behavior, not a bug.
	•	Shared storage is public within the artifact. Any user with access to this artifact can technically read all stored data, not just their own.
Taking this to production
If you want real accounts, encrypted passwords, persistent sessions, and true real-time chat, you’d need an actual backend (e.g. a small Node/Express or Firebase setup) instead of client-side storage. Happy to help plan that build if you want to go that route.