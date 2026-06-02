function distribusiData() {
  const ss = SpreadsheetApp.getActiveSpreadsheet();
  const planSheet = ss.getSheetByName("PlanDistribusi");
  const logSheet = ss.getSheetByName("LogDistribusi");
  const planData = planSheet.getDataRange().getValues();
  const sumberTracker = {};

  // Kolom PlanDistribusi
  const COL_STATUS=1, COL_TIPE=2, COL_MATERI=3, COL_CLUSTER=4;
  const COL_SUMBER_ID=5, COL_SUMBER_SHEET=6, COL_TARGET_ID=7, COL_TARGET_SHEET=8;
  const COL_BARIS_MULAI=9, COL_JUMLAH_AMBIL=10, COL_POSISI=11;
  const COL_TERAKHIR=12, COL_JUMLAH_RESET=13, COL_PLAN_END=14;

  // Daftar hari libur nasional Indonesia 2025 (SKB 3 Menteri)
  const holidays2025 = [
    '2025-01-01','2025-01-27','2025-01-29','2025-03-29',
    '2025-03-31','2025-04-01','2025-04-18','2025-04-20',
    '2025-05-01','2025-05-12','2025-05-29','2025-06-01',
    '2025-06-06','2025-06-27','2025-08-17','2025-09-05',
    '2025-12-25'
  ];
  
  const today = new Date();
  const isoToday = today.toISOString().slice(0,10);
  const isSunday = today.getDay() === 0;
  const isHoliday = holidays2025.indexOf(isoToday) !== -1;

  for (let i = 1; i < planData.length; i++) {
    const row = planData[i];
    const status = row[COL_STATUS];
    if (status !== "Aktif") continue;

    // Ambil data campaign
    const [tipeCampaign,materi,cluster,sumberId,sumberSheet,
           targetId,targetSheet,barisMulai,jumlahAmbil,posisiBaris] =
           [row[COL_TIPE],row[COL_MATERI],row[COL_CLUSTER],
            row[COL_SUMBER_ID],row[COL_SUMBER_SHEET],
            row[COL_TARGET_ID],row[COL_TARGET_SHEET],
            parseInt(row[COL_BARIS_MULAI]),parseInt(row[COL_JUMLAH_AMBIL]),
            parseInt(row[COL_POSISI])];

    // Cek plan berakhir
    const tanggalPlanAkhir = row[COL_PLAN_END];
    if (tanggalPlanAkhir instanceof Date && today > tanggalPlanAkhir) {
      planSheet.getRange(i+1, COL_STATUS+1).setValue("Non Aktif");
      continue;
    }

    // Skip jika hari Minggu atau libur nasional
    if (isSunday || isHoliday) {
      const alasan = isSunday ? "Minggu" : "Libur nasional";
      logSheet.appendRow([
        new Date(), tipeCampaign, materi, cluster,
        sumberId, sumberSheet, targetId, targetSheet,
        0, "Dibatalkan", `Distribusi dibatalkan: ${alasan}`
      ]);
      continue;
    }

    // Proses distribusi kampanye aktif dan bukan hari libur...
    try {
      const sumberSS = SpreadsheetApp.openById(sumberId);
      const dataSumber = sumberSS.getSheetByName(sumberSheet)
                        .getDataRange().getValues();
      const maxBaris = dataSumber.length;

      let posAwal = sumberTracker[sumberId + "|" + sumberSheet] || posisiBaris;
      let akhir = posAwal + jumlahAmbil - 1;

      if (akhir > maxBaris) {
        posAwal = row[COL_BARIS_MULAI];
        akhir = posAwal + jumlahAmbil - 1;
        const resetCell = planSheet.getRange(i+1, COL_JUMLAH_RESET+1);
        resetCell.setValue((resetCell.getValue()||0) + 1);
      }

      const dataAmbil = dataSumber.slice(posAwal-1, Math.min(akhir, maxBaris));
      if (dataAmbil.length === 0) throw new Error("Tidak ada data ambil.");

      const targetSheetObj = SpreadsheetApp.openById(targetId)
                                     .getSheetByName(targetSheet);
      targetSheetObj
        .getRange(barisMulai, 1, dataAmbil.length, dataAmbil[0].length)
        .setValues(dataAmbil);

      const posBaru = akhir + 1;
      sumberTracker[sumberId + "|" + sumberSheet] = posBaru;
      planSheet.getRange(i+1, COL_POSISI+1).setValue(posBaru);
      planSheet.getRange(i+1, COL_TERAKHIR+1).setValue(new Date());

      logSheet.appendRow([
        new Date(), tipeCampaign, materi, cluster,
        sumberId, sumberSheet, targetId, targetSheet,
        dataAmbil.length, "Sukses", ""
      ]);
    } catch (err) {
      logSheet.appendRow([
        new Date(), tipeCampaign, materi, cluster,
        sumberId, sumberSheet, targetId, targetSheet,
        0, "Gagal", err.message
      ]);
    }
  }
}
