# ✅ Username Login Implementation - Teacher & Guardian

## 📋 Overview

**Implemented**: Username-based login untuk role **Teacher** dan **Guardian**  
**Status**: ✅ Complete - Ready for migration and testing

## 🎯 Changes Summary

### Key Changes
1. **Teacher** login: Username (nama) - NOT NIP ❌
2. **Guardian** login: Username (nama) - NOT Email ❌  
3. Email & NIP tetap disimpan di database (data identitas)
4. Admin, Student, Principal tidak terpengaruh (tetap bisa pakai email/NIS/NIP)

## 📝 Files Modified

### Backend Files (7 files)

#### 1. ✅ `api/prisma/schema.prisma`
- Added `username String? @unique` to User model

#### 2. ✅ `api/prisma/migrations/20260619103036_add_username_field/migration.sql`
- Migration to add username column with unique index

#### 3. ✅ `api/src/lib/generateUsername.ts` (NEW)
- Function `generateUniqueUsername(fullName: string)`
- Logic: nama depan → tambah huruf nama belakang jika collision → tambah angka (last resort)
- Examples:
  - "Budi Santoso" → "budi"
  - "Budi Hartono" → "budiha" (if "budi" taken)
  - "Budi" (third) → "budi2" (if all combinations taken)

#### 4. ✅ `api/src/routes/auth.route.ts`
- Updated login query with exclusions:
  - Email: ONLY for roles OTHER than guardian
  - NipNis: ONLY for roles OTHER than teacher
  - Username: Available for all roles
  - UserCode: Available for all roles

#### 5. ✅ `api/src/routes/teachers.route.ts`
- Import `generateUniqueUsername`
- Generate username when creating teacher
- Store in User.username field
- Return `loginUsername` and `disambiguationHint` in response
- Race condition handling with timestamp fallback

#### 6. ✅ `api/src/routes/guardians.route.ts`
- Import `generateUniqueUsername`
- Generate username when creating guardian
- Store in User.username field
- Return `loginUsername` and `disambiguationHint` in response
- Race condition handling with timestamp fallback

### Frontend Files (3 files)

#### 7. ✅ `web/src/app/login/page.tsx`
- Updated label: "Username / Email / NIS / NIP"
- Updated placeholder: "Nama (Guru/Wali Murid), NIS, NIP, atau Email"
- Added helper text: "Guru & Wali Murid: gunakan Nama untuk login"

#### 8. ✅ `web/src/app/(admin)/teachers/page.tsx`
- Updated success message to show `loginUsername`
- Display format: `✅ Name (Subject) - Username Login: "username"`
- Warning to save username for first login

#### 9. ✅ `web/src/app/(admin)/guardians/page.tsx`
- Updated success message to show `loginUsername`
- Display format: `✅ Name (Wali dari Child) - Email: xxx - Username Login: "username"`
- Warning to save username for first login

## 🔧 Technical Details

### Login Query Logic

```typescript
const user = await prisma.user.findFirst({
  where: {
    OR: [
      { userCode: identifier }, // All roles
      { username: normalizedIdentifier }, // All roles
      // Email: Guardian EXCLUDED
      {
        AND: [
          { email: identifier },
          { role: { not: "guardian" } }
        ]
      },
      // NipNis: Teacher EXCLUDED
      {
        AND: [
          { nipNis: identifier },
          { role: { not: "teacher" } }
        ]
      }
    ]
  }
});
```

### Username Generation Logic

```
Input: "Budi Hartono Santoso"

Step 1: Try first name only
→ "budi"
→ Check DB: exists? → YES, try step 2

Step 2: Try first name + 2-4 chars from last name
→ "budiha" (budi + ha from hartono)
→ Check DB: exists? → NO, use "budiha" ✅

If all combinations taken:
Step 3: Add number (last resort)
→ "budi2", "budi3", etc.
```

### Race Condition Safety

Both teachers.route.ts and guardians.route.ts have fallback:

```typescript
catch (error: any) {
  if (error.code === "P2002" && error.meta?.target?.includes("username")) {
    // Retry once with timestamp
    const fallbackUsername = `${normalize(name)}${Date.now().toString().slice(-4)}`;
    // ... retry create with fallback
  }
}
```

## 🚀 Deployment Steps

### Step 1: Run Migration

```bash
cd api
npx prisma migrate deploy
```

**Expected output**:
```
✓ Applied migration 20260619103036_add_username_field
```

### Step 2: Generate Prisma Client

```bash
npx prisma generate
```

### Step 3: Restart Backend

```bash
npm run dev
```

### Step 4: Restart Frontend

```bash
cd ../web
npm run dev
```

## 🧪 Testing Checklist

### Backend Tests

- [ ] **Migration Success**
  ```bash
  cd api
  npx prisma migrate deploy
  # Should complete without errors
  ```

- [ ] **Generate Username Test**
  ```bash
  npx ts-node -e "
  import { generateUniqueUsername } from './src/lib/generateUsername';
  (async () => {
    console.log(await generateUniqueUsername('Budi Hartono'));
    console.log(await generateUniqueUsername('Siti Nurhaliza'));
  })();
  "
  ```

- [ ] **Teacher Creation** - API Endpoint
  ```bash
  POST /api/teachers
  Body: {
    "name": "Budi Hartono",
    "nip": "123456789",
    "email": "budi@test.com",
    "phone": "08123456789",
    "gender": "L"
  }
  
  Expected Response:
  {
    "success": true,
    "data": {
      "id": 1,
      "name": "Budi Hartono",
      "loginUsername": "budi" or "budiha",
      "disambiguationHint": "..."
    }
  }
  ```

- [ ] **Guardian Creation** - API Endpoint
  ```bash
  POST /api/guardians
  Body: {
    "name": "Siti Nurhaliza",
    "email": "siti@test.com",
    "phone": "08123456789",
    "address": "Jakarta",
    "occupation": "Ibu Rumah Tangga"
  }
  
  Expected Response:
  {
    "success": true,
    "data": {
      "id": 1,
      "name": "Siti Nurhaliza",
      "loginUsername": "siti",
      "disambiguationHint": "-" or "ChildName"
    }
  }
  ```

### Login Tests

- [ ] **Teacher Login - Username** ✅
  ```
  Identifier: budi (atau budiha)
  Password: G001 (default password)
  Expected: Success
  ```

- [ ] **Teacher Login - NIP** ❌
  ```
  Identifier: 123456789 (NIP)
  Password: G001
  Expected: FAIL - "Kredensial login salah"
  ```

- [ ] **Guardian Login - Username** ✅
  ```
  Identifier: siti
  Password: O001 (default password)
  Expected: Success
  ```

- [ ] **Guardian Login - Email** ❌
  ```
  Identifier: siti@test.com (email)
  Password: O001
  Expected: FAIL - "Kredensial login salah"
  ```

- [ ] **Admin Login - Email** ✅
  ```
  Identifier: admin@maleo.sch.id
  Password: xxx
  Expected: Success (not affected)
  ```

- [ ] **Student Login - NIS** ✅
  ```
  Identifier: 12345 (NIS)
  Password: xxx
  Expected: Success (not affected)
  ```

### Frontend Tests

- [ ] **Teachers Page**
  - Create new teacher "Budi Hartono"
  - Verify success message shows `Username Login: "budiha"`
  - Verify table shows teacher data with NIP still visible

- [ ] **Guardians Page**
  - Create new guardian "Siti Nurhaliza"
  - Verify success message shows `Username Login: "siti"`
  - Verify table shows guardian data with Email still visible

- [ ] **Login Page**
  - Verify placeholder: "Nama (Guru/Wali Murid), NIS, NIP, atau Email"
  - Verify helper text: "Guru & Wali Murid: gunakan Nama untuk login"
  - Test login with teacher username → Success
  - Test login with guardian username → Success

### Database Verification

```sql
-- Check username field exists and has data
SELECT id, name, role, username, email, nip_nis 
FROM users 
WHERE role IN ('teacher', 'guardian')
ORDER BY created_at DESC 
LIMIT 10;

-- Expected: username column populated for new teachers/guardians
```

## ⚠️ Important Notes

### Data Integrity
- ✅ NIP field **NOT REMOVED** from Teacher table
- ✅ Email field **NOT REMOVED** from Guardian table
- ✅ NIP still visible in admin teachers page
- ✅ Email still visible in admin guardians page
- ✅ Only LOGIN behavior changed

### Existing Users
- ⚠️ **Existing teachers/guardians**: username will be NULL
- 📝 **Action required**: Generate usernames for existing users
- 📝 **Script needed**: Bulk update script (see below)

### Roles NOT Affected
- ✅ Admin: still uses email for login
- ✅ Student: still uses NIS for login
- ✅ Principal: still uses email/code for login

## 📊 Impact Analysis

### Before Implementation
| Role | Login Method |
|------|-------------|
| Teacher | NIP or Email |
| Guardian | Email |
| Admin | Email |
| Student | NIS |

### After Implementation
| Role | Login Method |
|------|-------------|
| Teacher | **Username (nama)** or UserCode |
| Guardian | **Username (nama)** or UserCode |
| Admin | Email (unchanged) |
| Student | NIS (unchanged) |

## 🔄 Migration Script for Existing Users

**TODO**: Create script to generate usernames for existing teachers and guardians

```typescript
// api/scripts/generate-existing-usernames.ts
import { prisma } from "../src/lib/prisma";
import { generateUniqueUsername } from "../src/lib/generateUsername";

async function migrateExistingUsers() {
  // Get all teachers without username
  const teachers = await prisma.user.findMany({
    where: {
      role: "teacher",
      username: null
    },
    include: { teacher: true }
  });

  for (const user of teachers) {
    const username = await generateUniqueUsername(user.name);
    await prisma.user.update({
      where: { id: user.id },
      data: { username }
    });
    console.log(`Teacher: ${user.name} → ${username}`);
  }

  // Get all guardians without username
  const guardians = await prisma.user.findMany({
    where: {
      role: "guardian",
      username: null
    }
  });

  for (const user of guardians) {
    const username = await generateUniqueUsername(user.name);
    await prisma.user.update({
      where: { id: user.id },
      data: { username }
    });
    console.log(`Guardian: ${user.name} → ${username}`);
  }
}

migrateExistingUsers().then(() => {
  console.log("Migration complete!");
  process.exit(0);
});
```

**Run**:
```bash
cd api
npx ts-node scripts/generate-existing-usernames.ts
```

## ✅ Success Criteria

- [x] Schema updated with username field
- [x] Migration file created
- [x] generateUsername utility created
- [x] Auth route updated with exclusions
- [x] Teachers route generates username
- [x] Guardians route generates username
- [x] Login page updated with new labels
- [x] Teachers page shows username in success message
- [x] Guardians page shows username in success message
- [ ] **Migration deployed to database**
- [ ] **Backend tests passing**
- [ ] **Frontend tests passing**
- [ ] **Existing users migrated (if any)**

## 🎉 Expected User Experience

### For Admin (Creating Teacher/Guardian)
1. Fill form dengan nama lengkap
2. Klik "Simpan"
3. Muncul success message: 
   ```
   ✅ Budi Hartono (Matematika) berhasil ditambahkan!
   
   Username Login: "budiha"
   
   ⚠️ Catat dan informasikan username ini secara langsung,
   karena ini yang akan dipakai untuk login pertama kali.
   ```

### For Teacher/Guardian (First Login)
1. Buka halaman login
2. Lihat placeholder: "Nama (Guru/Wali Murid), NIS, NIP, atau Email"
3. Masukkan nama depan: "budi" or "budiha"
4. Masukkan password default: G001 atau O001
5. Login berhasil ✅
6. Diminta ganti password

## 🐛 Troubleshooting

### Issue: Migration fails with "column already exists"
```bash
# Check if column exists
psql -U postgres -d maleo -c "\d users"

# If username column exists, mark migration as applied
npx prisma migrate resolve --applied 20260619103036_add_username_field
```

### Issue: generateUsername import error
```bash
# Regenerate Prisma client
npx prisma generate

# Restart TypeScript server in IDE
```

### Issue: Login fails for teacher with username
- Check: Is username field populated in database?
- Check: Is auth.route.ts updated with exclusion logic?
- Check: Console log the query result in auth.route.ts

### Issue: Success message doesn't show username
- Check: Does API response include `loginUsername`?
- Check: Is frontend parsing response.data correctly?

---

**Status**: ✅ Implementation Complete  
**Next Step**: Run migration and test  
**Priority**: HIGH (blocks teacher/guardian login)
