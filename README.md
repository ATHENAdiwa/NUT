// หน้าต่างตั๋ว (เปิดได้เฉพาะตั๋วของตัวเอง)
function openTicket(code) {
  const user = currentUser();
  const b = load("st_bookings", []).find(x => x.code === code);
  if (!user || !b || b.email !== user.email) return;
  const t = tours.find(item => item.id === b.tourId);
  ticketBox.innerHTML = `
    <p class="ticket-label">ตั๋วทัวร์${t.country}</p>
    <h3>${t.title}</h3>
    <div class="ticket-code">${b.code}</div>
    <div class="barcode" aria-hidden="true"></div>
    <dl class="ticket-info">
      <div><dt>ผู้จอง</dt><dd>${esc(b.name)}</dd></div>
      <div><dt>ผู้เดินทาง</dt><dd>${b.people} คน</dd></div>
      <div><dt>ระยะเวลา</dt><dd>${t.days} วัน</dd></div>
      <div><dt>ราคารวม</dt><dd>${formatPrice(b.total)}</dd></div>
      <div><dt>วันที่จอง</dt><dd>${formatDate(b.date)}</dd></div>
    </dl>
    <div class="dialog-actions"><button class="btn ghost" data-close>ปิด</button></div>
  `;
  ticketDialog.style.setProperty("--theme", t.color);
  if (!ticketDialog.open) ticketDialog.showModal();
}
