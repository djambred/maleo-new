# Manual Testing Checklist - Task 8 & Task 10

**Date**: June 13, 2026  
**Project**: Web SIAKAD - Material Access Tracking Bugfix  
**Purpose**: Manual verification of frontend tracking and end-to-end flows

---

## Pre-Testing Setup

### 1. Environment Preparation

- [ ] Backend server running: `cd api && npm run dev` (port 4000)
- [ ] Frontend server running: `cd web && npm run dev` (port 3000)
- [ ] Database seeded: `cd api && npm run db:seed`
- [ ] Browser DevTools ready (Network tab, Console tab)
- [ ] Database client ready (for SQL queries)

### 2. Test Data Preparation

Create test data via frontend or SQL:

```sql
-- Verify test teacher exists
SELECT id, name FROM teachers WHERE email = 'guru.dummy@maleo.sch.id';

-- Create test class if needed
INSERT INTO classes (name, level, homeroom_teacher_id)
VALUES ('7A Test', 7, <teacher_id>)
ON CONFLICT (name) DO NOTHING;

-- Create test student
INSERT INTO users (name, email, password, role, user_code)
VALUES ('Test Student', 'student@test.com', '$2a$10$hashed', 'student', 'STUDENT001')
RETURNING id;

INSERT INTO students (nis, name, class_id)
VALUES ('TEST001', 'Test Student', <class_id>)
RETURNING id;

-- Link user to student
UPDATE users SET student_id = <student_id> WHERE id = <user_id>;
```

Or use existing student account from seeded data.

---

## Task 8: Frontend Bug Verification

### Objective
Verify that clicking "Buka" on LMS materials now calls the correct tracking endpoint.

### Test 8.1: LMS Material Access - Network Request Verification

**Steps**:

1. [ ] Login as student
   - Email: (use test student email)
   - Password: (use test student password)
   
2. [ ] Navigate to Materials page (`/hub/materials`)
   
3. [ ] Open Browser DevTools
   - Press F12
   - Go to **Network** tab
   - Click "Clear" to clear existing requests
   - Enable "Preserve log"
   
4. [ ] Create or verify LMS module exists with at least one material
   - If no module exists, login as teacher first and create one
   - Ensure module is published
   - Ensure at least one material (PDF, video, or link) exists
   
5. [ ] As student, expand module → expand session → click on material
   
6. [ ] Click "Buka" button on LMS material
   
7. [ ] **VERIFY in Network tab**:
   - [ ] Request appears: `POST /api/lms/materials/{id}/access`
   - [ ] Status: `200 OK`
   - [ ] Response body: `{ "success": true, "message": "Akses berhasil direkam." }`
   
8. [ ] **VERIFY material opens**:
   - [ ] New tab/window opens with material
   - [ ] Material is viewable/downloadable
   
9. [ ] **VERIFY database**:
   ```sql
   SELECT * FROM student_material_access 
   ORDER BY accessed_at DESC 
   LIMIT 5;
   ```
   - [ ] New record exists with current timestamp
   - [ ] studentId matches test student
   - [ ] materialId matches clicked material

**Expected Result**: ✅ LMS tracking endpoint called, material opens, database record created

**Screenshot Location**: `screenshots/task8-lms-tracking-network.png`

---

### Test 8.2: LMS Repeat Access - Timestamp Update

**Steps**:

1. [ ] Continue from Test 8.1 (already logged in as student)
   
2. [ ] Return to Materials page
   
3. [ ] Click "Buka" on THE SAME material again
   
4. [ ] **VERIFY in Network tab**:
   - [ ] Request appears: `POST /api/lms/materials/{id}/access`
   - [ ] Status: `200 OK` (no error)
   
5. [ ] **VERIFY database**:
   ```sql
   SELECT id, student_id, material_id, accessed_at 
   FROM student_material_access 
   WHERE student_id = <test_student_id> 
   AND material_id = <test_material_id>;
   ```
   - [ ] Still only ONE record (no duplicate)
   - [ ] `accessed_at` timestamp updated to most recent access

**Expected Result**: ✅ Repeat access succeeds, timestamp updated, no duplicates

---

### Test 8.3: RPS Material Tracking - Preservation Check

**Steps**:

1. [ ] Login as student (if not already)
   
2. [ ] Navigate to Materials page (`/hub/materials`)
   
3. [ ] Look for RPS section (if RPS materials exist in your test data)
   - If no RPS exists, skip this test OR create RPS material as teacher first
   
4. [ ] Open DevTools Network tab, clear log
   
5. [ ] Click "Buka" on RPS material
   
6. [ ] **VERIFY in Network tab**:
   - [ ] Request appears: `POST /api/rps/student/materials/{id}/access`
   - [ ] Status: `200 OK`
   - [ ] (NOT calling LMS endpoint)
   
7. [ ] **VERIFY database**:
   ```sql
   SELECT * FROM rps_material_access 
   WHERE student_id = <test_student_id> 
   ORDER BY accessed_at DESC 
   LIMIT 5;
   ```
   - [ ] Record created in `rps_material_access` table
   - [ ] (NOT in `student_material_access` table)

**Expected Result**: ✅ RPS tracking still uses RPS endpoint (preserved)

---

### Test 8.4: Console Error Check

**Steps**:

1. [ ] Open DevTools Console tab
   
2. [ ] Clear console
   
3. [ ] Click "Buka" on LMS material
   
4. [ ] **VERIFY in Console**:
   - [ ] No errors logged
   - [ ] No warnings about failed tracking
   - [ ] (Silent failure working correctly)

**Expected Result**: ✅ No console errors

---

### Test 8.5: Tracking Failure Scenario - Non-Blocking

**Purpose**: Verify material still opens even if tracking fails

**Steps**:

1. [ ] **STOP the backend server** (simulate API failure)
   - Press Ctrl+C in backend terminal
   
2. [ ] As student (frontend still running), click "Buka" on material
   
3. [ ] **VERIFY**:
   - [ ] Network shows failed request (red in DevTools)
   - [ ] Error logged in console: "Failed to track LMS material access"
   - [ ] **Material still opens** in new tab (non-blocking)
   
4. [ ] **RESTART backend server**: `npm run dev`

**Expected Result**: ✅ Material opens despite tracking failure (silent failure working)

---

## Task 10: Integration Testing (Manual Components)

### Test 10.1: End-to-End Student Flow

**Complete user journey**:

1. [ ] **Login as student**
   - Email: (student account)
   - Password: (student password)
   
2. [ ] **Navigate to Materials page** (`/hub/materials`)
   
3. [ ] **Browse modules**
   - [ ] Modules list visible
   - [ ] Can expand/collapse modules
   
4. [ ] **Select module → Select session**
   - [ ] Sessions list visible
   - [ ] Can view session details
   
5. [ ] **View materials list**
   - [ ] Materials displayed with icons
   - [ ] Material types shown (PDF, video, link)
   - [ ] "Buka" button visible
   
6. [ ] **Click "Buka" on first material**
   - [ ] Tracking endpoint called (verify in Network tab)
   - [ ] Material opens in new tab
   - [ ] Database record created
   
7. [ ] **Click same material again**
   - [ ] No error
   - [ ] Material opens again
   - [ ] Timestamp updated (not duplicate)
   
8. [ ] **Click different material**
   - [ ] Tracking endpoint called
   - [ ] Separate database record created
   - [ ] Material opens

**Expected Result**: ✅ Complete flow works seamlessly

---

### Test 10.2: Teacher Analytics Verification

**Verify teacher can see accurate access counts**:

1. [ ] Ensure test student accessed 2-3 materials (from Test 10.1)
   
2. [ ] **Logout and login as teacher**
   - Email: `guru.dummy@maleo.sch.id`
   - Password: `GuruMaster123!`
   
3. [ ] **Navigate to Materials page** (teacher view)
   
4. [ ] **Expand module → expand session**
   
5. [ ] **VERIFY materials display**:
   - [ ] Material cards show access count
   - [ ] Count matches database query:
     ```sql
     SELECT m.id, m.title, COUNT(sma.id) as access_count
     FROM session_materials m
     LEFT JOIN student_material_access sma ON m.id = sma.material_id
     GROUP BY m.id, m.title;
     ```
   - [ ] "Diakses X kali" text appears

**Expected Result**: ✅ Teacher sees accurate access counts

---

### Test 10.3: Multiple Students - Same Material

**Verify different students create separate records**:

1. [ ] **Login as Student 1**
   
2. [ ] Access Material A (note material ID)
   
3. [ ] **Verify database**:
   ```sql
   SELECT COUNT(*) FROM student_material_access WHERE material_id = <material_a_id>;
   ```
   - [ ] Count = 1
   
4. [ ] **Logout and login as Student 2** (or use incognito window)
   
5. [ ] Access same Material A
   
6. [ ] **Verify database**:
   ```sql
   SELECT COUNT(*) FROM student_material_access WHERE material_id = <material_a_id>;
   ```
   - [ ] Count = 2 (separate records for each student)

**Expected Result**: ✅ Separate records per student

---

### Test 10.4: Cross-Browser Testing

**Test in different browsers**:

- [ ] **Chrome/Edge**
  - [ ] LMS tracking works
  - [ ] Materials open correctly
  
- [ ] **Firefox**
  - [ ] LMS tracking works
  - [ ] Materials open correctly
  
- [ ] **Safari** (if available)
  - [ ] LMS tracking works
  - [ ] Materials open correctly

**Expected Result**: ✅ Works consistently across browsers

---

### Test 10.5: Mobile Responsive Testing

**Test on mobile viewport**:

1. [ ] Open DevTools → Toggle device toolbar (Ctrl+Shift+M)
   
2. [ ] Set to mobile device (e.g., iPhone 12)
   
3. [ ] Navigate as student to Materials page
   
4. [ ] **VERIFY**:
   - [ ] Layout responsive
   - [ ] "Buka" button accessible
   - [ ] Clicking "Buka" works
   - [ ] Tracking endpoint called
   
5. [ ] Test on actual mobile device (if available)

**Expected Result**: ✅ Works on mobile devices

---

## Task 10.4: Final Manual Testing Checklist

### Database Verification

```sql
-- Check unique constraint is active
SELECT constraint_name, constraint_type 
FROM information_schema.table_constraints 
WHERE table_name = 'student_material_access';
-- Should show UNIQUE constraint on (student_id, material_id)

-- Check for duplicate records (should return 0 rows)
SELECT student_id, material_id, COUNT(*) 
FROM student_material_access 
GROUP BY student_id, material_id 
HAVING COUNT(*) > 1;

-- Verify tracking data exists
SELECT COUNT(*) FROM student_material_access;
-- Should be > 0 after testing

-- Verify timestamps are recent
SELECT * FROM student_material_access 
ORDER BY accessed_at DESC 
LIMIT 10;
-- Should show recent timestamps from testing
```

- [ ] Unique constraint active
- [ ] Zero duplicate records
- [ ] Tracking data exists
- [ ] Timestamps accurate

### Backend Verification

- [ ] No console errors in backend terminal
- [ ] Tracking endpoint logs show success
- [ ] Example log: `[LMS] trackAccess: success for student <id>, material <id>`

### Frontend Verification

- [ ] No console errors in browser
- [ ] DevTools Network tab shows correct endpoints
- [ ] Materials open without issues
- [ ] UI responsive and functional

### Performance Verification

- [ ] Tracking request completes quickly (<100ms)
- [ ] Material opening not delayed by tracking
- [ ] No noticeable lag in UI

---

## Regression Checks

### Verify nothing broke:

- [ ] RPS material tracking still works
- [ ] Teacher can create/edit modules
- [ ] Teacher can upload materials
- [ ] Guardian can view student materials
- [ ] File download/preview works
- [ ] Material types display correctly (PDF, video, link icons)

---

## Final Verification Checklist

### Core Requirements

- [ ] **2.1**: Upsert pattern works (no P2002 errors)
- [ ] **2.2**: Timestamps update on repeat access
- [ ] **2.3**: Unique constraint enforced
- [ ] **2.4**: Frontend calls `/lms/materials/:id/access`
- [ ] **2.5**: RPS tracking preserved (calls `/rps/...`)
- [ ] **2.6**: Tracking failure doesn't block material access
- [ ] **2.7**: Complete tracking flow works end-to-end

### Preservation Requirements

- [ ] **3.1**: First access creates record
- [ ] **3.2**: Teacher analytics unchanged
- [ ] **3.3**: Authorization checks enforced
- [ ] **3.4**: Analytics features work
- [ ] **3.5**: RPS tracking unchanged
- [ ] **3.6**: File serving non-blocking

---

## Test Results Summary

**Date Tested**: _______________  
**Tested By**: _______________  
**Environment**: ☐ Development ☐ Staging ☐ Production

### Results:

| Test Section | Pass | Fail | Notes |
|--------------|------|------|-------|
| Task 8.1 - Network Request | ☐ | ☐ | |
| Task 8.2 - Repeat Access | ☐ | ☐ | |
| Task 8.3 - RPS Preservation | ☐ | ☐ | |
| Task 8.4 - Console Errors | ☐ | ☐ | |
| Task 8.5 - Silent Failure | ☐ | ☐ | |
| Task 10.1 - E2E Flow | ☐ | ☐ | |
| Task 10.2 - Teacher Analytics | ☐ | ☐ | |
| Task 10.3 - Multiple Students | ☐ | ☐ | |
| Task 10.4 - Cross-Browser | ☐ | ☐ | |
| Task 10.5 - Mobile | ☐ | ☐ | |

### Overall Status:

☐ **PASS** - All tests passed, ready for production  
☐ **CONDITIONAL PASS** - Minor issues found (list below)  
☐ **FAIL** - Critical issues found (list below)

### Issues Found:

1. ________________________________________________
2. ________________________________________________
3. ________________________________________________

### Screenshots Captured:

- [ ] LMS tracking network request
- [ ] Database records after testing
- [ ] Teacher analytics view
- [ ] Mobile responsive view
- [ ] Cross-browser testing results

---

## Sign-Off

**Developer**: ______________________ Date: ________  
**QA Tester**: ______________________ Date: ________  
**Project Manager**: ________________ Date: ________

**Approved for Production**: ☐ Yes ☐ No

---

## Next Steps After Manual Testing

1. If all tests pass → Proceed to **Task 11: Final Checkpoint**
2. If issues found → Document and fix before proceeding
3. Capture screenshots for documentation
4. Update progress report with manual testing results
