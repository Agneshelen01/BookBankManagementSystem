# Book Bank Management System

## Requirements
- Node.js (LTS recommended)
- npm

## Start the backend
Open a terminal in `backend` and run:
```bash
npm install
npm run dev
```
Before `npm run dev`, set a private signing key in the same terminal. For PowerShell:
```powershell
$bytes = New-Object byte[] 32
[Security.Cryptography.RandomNumberGenerator]::Create().GetBytes($bytes)
$env:JWT_SECRET = [Convert]::ToBase64String($bytes)
```
The API runs at `http://localhost:5000` and creates/updates `backend/bookbank.db` automatically. Keep `JWT_SECRET` private and do not commit it.

## Start the frontend
Open a second terminal in `frontend` and run:
```bash
npm install
npm run dev
```
Open the local URL printed by Vite, usually `http://localhost:5173`.

## Optional demo accounts
On an empty database, the backend creates student, librarian, and admin demo accounts only when `DEMO_PASSWORD` is set before startup. Set it to a private password in the same terminal; all three demo accounts use it. Their usernames are `student01`, `librarian01`, and `admin01`. Do not commit the password. Without `DEMO_PASSWORD`, no demo accounts are created; students can create accounts through registration.

## Student registration
Open `/register` from the login page. New students enter their name, email, department, year and password. The backend validates the fields, prevents duplicate email addresses, hashes passwords with bcrypt, and generates a student login ID such as `STU001`. The ID is shown in the success message; use it as the username on the login page. Public registration always creates a student account; it cannot create librarian/admin accounts.

Existing SQLite databases are migrated automatically to add the registration fields without deleting existing users or books.


## Initial inventory and admin user list
- On a fresh database, the sample book catalogue is preloaded and every copy starts as available.
- Issued books, reservations, and transaction history start at zero because the transactions and reservations tables are empty.
- Returning a book removes it from the active-issued count; historical issue/return records remain in transaction history.
- Admins have a **Manage Users** page to view all accounts or filter by Student, Librarian, or Administrator. Public registration can create student accounts only.
- The admin demo account is available when the optional demo accounts are initialized.

If you want to reset a local demo database completely, stop the backend first and delete `backend/bookbank.db`, then restart `npm run dev`. This recreates the initial full-availability catalogue and creates demo accounts only if `DEMO_PASSWORD` is set. Do not do this if you need to keep registered users or circulation records.

## Catalogue and filters
- The starter catalogue contains 6 sample titles with 61 total copies (12 + 8 + 10 + 15 + 9 + 7). “61 books” means physical copies, not 61 distinct titles.
- Student Check Availability and staff Book Inventory pages show titles, total copies, available copies and stock status. Search by title, author, ISBN or book ID; filter by category and availability.
- The Reserve button is shown only when a title has zero available copies. Students cannot reserve a title while copies are available.
