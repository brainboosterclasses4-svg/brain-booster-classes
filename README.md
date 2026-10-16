# Brain Booster Classes — Starter v1

A beginner-friendly React + Vite + Firebase learning portal starter. Includes:
- Email/password student sign-up and sign-in
- Teacher/admin workspace (after secure Firebase setup)
- Class-wise lesson records with video and PDF links
- Class-wise homework and text/link submissions
- Responsive mobile layout

## Before you start
Install Node.js LTS and VS Code. Create a Firebase project. This starter stores links to videos/PDFs; it does not upload large video files to Firebase.

## 1. Run locally
1. Extract this ZIP.
2. Open the extracted `brain-booster-classes` folder in VS Code.
3. Open Terminal in VS Code and run:
   ```bash
   npm install
   ```
4. Copy `.env.example` to `.env.local`.
5. Add the Firebase Web App configuration values to `.env.local`.
6. Run:
   ```bash
   npm run dev
   ```
7. Open the local URL printed by Vite.

## 2. Configure Firebase
1. In Firebase Console, create a project.
2. Add a Web App and copy its config values into `.env.local`.
3. Authentication → Sign-in method → enable Email/Password.
4. Build → Firestore Database → Create database.
5. In Firebase Console → Authentication → Users, click **Add user** and create your teacher account using your teacher email and a strong password.
6. Copy the UID shown for that teacher account.
7. Open `firestore.rules`; replace `ADMIN_UID_HERE` with that exact UID (keep the quotation marks).
8. Publish those rules in Firestore → Rules.
9. In Firestore → Data, create collection `users` if needed, and a document whose ID is your teacher UID. Set these fields:
   - `uid` (string): your exact UID
   - `email` (string): your teacher email in lowercase
   - `displayName` (string): your display name
   - `role` (string): `admin`
   - `approved` (boolean): `true`
   - `classId` (string): `admin`
   - `createdAt` (timestamp): use the console timestamp value if desired, or omit it
10. Sign in again. The admin dashboard should now be visible.

### Important admin setup note
Create the teacher's Auth account directly in Firebase Console first, then add the matching `users/{teacherUID}` profile manually after publishing the rules. This avoids the default locked Firestore rules blocking the initial setup. Do not let students choose an admin role. Never publish rules with `ADMIN_UID_HERE` unchanged.

## 3. Approve students
This v1 deliberately makes new student accounts pending. To approve a student:
1. The student signs up with their email.
2. In Firebase Authentication, find the student and copy their UID.
3. In Firestore `users`, open the document with that UID.
4. Set `approved` to `true` and set `classId` to exactly one of: `Class 6`, `Class 7`, `Class 8`, `Class 9`, `Class 10`, `Class 11`, `Class 12`.
5. Do not change `role` from `student`.

## 4. Publish a lesson
In the admin dashboard, enter title, class, subject, chapter, a YouTube recording URL and/or a Google Drive PDF URL. Confirm the resource's sharing settings before publishing.

- YouTube: an Unlisted link is convenient, but anyone with the link may share it. It is not strict privacy.
- Drive: set the file to the intended audience. A Drive link is not private merely because it is placed behind a login page.
- This starter saves resource links, not the actual file bytes.

## 5. Publish homework
Choose a class and subject, write instructions and a due date, and publish. Students can submit a written answer or a link to their work. This first version does not yet provide file uploads, grading, notifications, attendance, or a submissions review page.

## 6. Build for deployment
Run `npm run build`. The production files are created in `dist/`. Choose a host whose terms permit a coaching/learning business. GitHub Pages' terms exclude sites primarily intended to facilitate commercial transactions or commercial software services, so do not assume it is suitable for a business portal. Check the current terms of your chosen host.

## Security checklist before inviting real students
- Replace the admin UID placeholder and publish rules.
- Test with a teacher account and a separate student account.
- Confirm student A cannot read another class's lessons.
- Confirm student accounts cannot create/edit lessons or homework.
- Do not place service-account credentials or private keys in frontend code.
- Do not store sensitive personal information unnecessarily.
- Protect student privacy and get appropriate parent/guardian permission where required.

## Common issues
- `Missing or insufficient permissions`: check that rules are published and the signed-in teacher UID exactly matches the UID in rules.
- Firebase not configured: ensure `.env.local` exists in the project root, all values are filled, and restart `npm run dev`.
- `auth/operation-not-allowed`: enable Email/Password in Firebase Authentication.
- Blank page after changing env: stop the server with Ctrl+C and restart `npm run dev`.
