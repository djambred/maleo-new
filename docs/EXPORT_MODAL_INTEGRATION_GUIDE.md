# Export Attendance Modal - Integration Guide

## ✅ Build Status: SUCCESS

Build completed successfully with no errors!

```
✓ Compiled successfully
✓ Linting and checking validity of types
✓ Collecting page data
✓ Generating static pages (43/43)
```

## 🎯 Fixed Issues

1. **Import Path Fixed** (Line 7)
   - ❌ Before: `import apiService from "@/services/api.service"`
   - ✅ After: `import { apiService } from "@/services/apiService"`

2. **API Method Fixed** (Line 52)
   - ❌ Before: `await apiService.get("/classes")`
   - ✅ After: `await apiService.getAll("/classes")`

## 📦 Component Location

- **File**: `web/src/components/ExportAttendanceModal.tsx`
- **Status**: Ready to use ✅
- **Dependencies**: All installed ✅

## 🚀 Integration Steps

### Option 1: Add to Student Attendance Page

File: `web/src/app/attendances/page.tsx` (or similar)

```typescript
import { useState } from "react";
import ExportAttendanceModal from "@/components/ExportAttendanceModal";

export default function AttendancesPage() {
  const [showExportModal, setShowExportModal] = useState(false);

  return (
    <div>
      {/* Existing content */}
      
      {/* Add Export Button */}
      <button
        onClick={() => setShowExportModal(true)}
        className="flex items-center gap-2 rounded-lg bg-gradient-to-r from-[#4BB8FA] to-[#1591DC] px-4 py-2 font-bold text-white shadow-lg"
      >
        <Download className="h-5 w-5" />
        Export Excel
      </button>

      {/* Add Modal */}
      <ExportAttendanceModal
        isOpen={showExportModal}
        onClose={() => setShowExportModal(false)}
      />
    </div>
  );
}
```

### Option 2: Add to Teacher Attendance Page

File: `web/src/app/teacher-attendances/page.tsx`

```typescript
import { useState } from "react";
import ExportAttendanceModal from "@/components/ExportAttendanceModal";

export default function TeacherAttendancesPage() {
  const [showExportModal, setShowExportModal] = useState(false);

  return (
    <div>
      {/* Existing content */}
      
      {/* Add Export Button */}
      <button
        onClick={() => setShowExportModal(true)}
        className="flex items-center gap-2 rounded-lg bg-gradient-to-r from-[#4BB8FA] to-[#1591DC] px-4 py-2 font-bold text-white"
      >
        <FileSpreadsheet className="h-5 w-5" />
        Export Rekap
      </button>

      {/* Add Modal */}
      <ExportAttendanceModal
        isOpen={showExportModal}
        onClose={() => setShowExportModal(false)}
      />
    </div>
  );
}
```

### Option 3: Add to Hub Teacher Dashboard

File: `web/src/app/hub/attendance/page.tsx`

```typescript
"use client";

import { useState } from "react";
import ExportAttendanceModal from "@/components/ExportAttendanceModal";
import { Download } from "lucide-react";

export default function HubAttendancePage() {
  const [showExportModal, setShowExportModal] = useState(false);

  return (
    <div className="p-6">
      <div className="mb-6 flex items-center justify-between">
        <h1 className="text-2xl font-bold">Absensi</h1>
        
        <button
          onClick={() => setShowExportModal(true)}
          className="flex items-center gap-2 rounded-lg bg-gradient-to-r from-[#4BB8FA] to-[#1591DC] px-4 py-2.5 font-bold text-white shadow-lg shadow-[#4BB8FA]/30 transition-all hover:shadow-xl"
        >
          <Download className="h-5 w-5" />
          Export Excel
        </button>
      </div>

      {/* Existing attendance content */}

      <ExportAttendanceModal
        isOpen={showExportModal}
        onClose={() => setShowExportModal(false)}
      />
    </div>
  );
}
```

## 🧪 Testing the Feature

### 1. Test Backend Endpoints

```bash
# Test Student Export
curl -X GET "http://localhost:4000/api/export/attendance/student?classId=1&startDate=2026-01-01&endDate=2026-01-31" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  --output student_attendance.xlsx

# Test Teacher Export
curl -X GET "http://localhost:4000/api/export/attendance/teacher?month=1&year=2026" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  --output teacher_attendance.xlsx
```

### 2. Manual Testing Checklist

- [ ] **Modal Opens**: Click export button, modal appears
- [ ] **Toggle Works**: Switch between "Rekap Siswa" and "Rekap Tutor"
- [ ] **Student Export**:
  - [ ] Classes dropdown loads
  - [ ] Date pickers work
  - [ ] Validation shows errors for missing fields
  - [ ] Download generates Excel file
  - [ ] Excel matches template format
- [ ] **Teacher Export**:
  - [ ] Month dropdown works
  - [ ] Year input accepts numbers
  - [ ] Download generates Excel file
  - [ ] Week columns (M-1, M-2, M-3, M-4) correct
  - [ ] Teacher names show as "Name (Subject)"
- [ ] **Loading States**: Spinner shows during download
- [ ] **Error Handling**: Toast notifications show errors
- [ ] **Close Modal**: X button and Cancel button work

## 📊 API Endpoints

### Student Attendance Export

```
GET /api/export/attendance/student
Query Params:
  - classId: number (required)
  - startDate: string (YYYY-MM-DD, required)
  - endDate: string (YYYY-MM-DD, required)

Response: Excel file (.xlsx)
Filename: Rekap_Siswa_{ClassName}_{timestamp}.xlsx
```

### Teacher Attendance Export

```
GET /api/export/attendance/teacher
Query Params:
  - month: string (1-12, required)
  - year: number (required)

Response: Excel file (.xlsx)
Filename: Rekap_Tutor_{MonthName}_{Year}.xlsx
```

## 🎨 Design System

Modal uses **Sky-Ocean Blue** theme:
- Primary Gradient: `from-[#4BB8FA] to-[#1591DC]`
- Text: White on gradient buttons
- Hover: Enhanced shadow effects
- Loading: Animated spinner (Loader2)

## ⚙️ Environment Variables

Ensure `.env.local` has:

```env
NEXT_PUBLIC_API_URL=http://localhost:4000/api
```

## 📝 Notes for Bu Triya (Kepala Sekolah)

1. **Student Report**: Shows attendance summary with columns: NIS, Name, Present (Hadir), Sick (Sakit), Permission (Izin), Absent (Alpa)

2. **Teacher Report**: Shows weekly attendance in format:
   - M-1: Week 1 (Days 1-7)
   - M-2: Week 2 (Days 8-14)
   - M-3: Week 3 (Days 15-21)
   - M-4: Week 4 (Days 22-31)

3. **Format**: Excel files match the provided templates exactly

## ✅ Next Steps

1. Choose where to integrate the modal (attendance pages)
2. Add the export button to the UI
3. Test both student and teacher exports
4. Verify Excel format matches Bu Triya's requirements
5. Deploy to production

## 🐛 Troubleshooting

**Modal doesn't open?**
- Check if `useState` is imported
- Verify button onClick calls `setShowExportModal(true)`

**Classes not loading?**
- Check API endpoint `/api/classes` is working
- Verify authentication token is valid

**Download fails?**
- Check browser console for errors
- Verify backend API is running on port 4000
- Ensure `NEXT_PUBLIC_API_URL` is set correctly

**Excel format wrong?**
- Check backend service: `api/src/services/attendance-export.service.ts`
- Verify data mappings match database schema
