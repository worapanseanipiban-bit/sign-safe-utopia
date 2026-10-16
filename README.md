# 🛡️ Sign Safe: U-Topia Transaction Guardian

**Live Demo:** https://worapanseanipiban-bit.github.io/sign-safe-utopia/
**Network:** Avalanche Fuji Testnet (Chain ID: `43113`)
**Demo Video:** <ใส่ลิงก์วิดีโอ>

Sign Safe ให้ U-Bot ตรวจธุรกรรมก่อนที่ผู้ใช้จะกดเซ็นใน MetaMask เพื่อลดการเสียเงินจากการโอนผิด address, โอนเข้า burn address หรือโอนเข้าสัญญาที่ไม่ได้ตั้งใจใน U-Topia Ecosystem

---

## ❗ ปัญหา
- ผู้ใช้ Web3 มักเสียเงินจากการเซ็นธุรกรรมโดยไม่รู้ว่าปลายทางคืออะไร
- Address poisoning / พิมพ์ address ผิด / โอนเข้า null address ทำให้เงินหายถาวร
- หน้าต่าง MetaMask แสดงข้อมูลดิบ ซึ่งผู้ใช้ทั่วไปอ่านไม่ออก

## ✅ วิธีแก้
Sign Safe มีขั้นตอน **Analyze → Review → Sign**:
1. **Connect Wallet** — เชื่อม MetaMask แล้วสลับ (หรือเพิ่ม) เครือข่าย Avalanche Fuji ให้อัตโนมัติ
2. **U-Bot Analyze** — อ่านข้อมูลจาก Fuji แบบ on-chain แล้วให้คะแนนความเสี่ยง 0–100
3. **Sign & Send** — ส่งธุรกรรมจริงบน Fuji พร้อมลิงก์ไป SnowTrace และรอ confirmation

### สิ่งที่ U-Bot ตรวจ
| การตรวจ | ผลลัพธ์ |
|---|---|
| เครือข่ายไม่ใช่ Fuji (43113) | ⛔ บล็อก |
| ปลายทางเป็น null/burn address (`0x000…0`, `0x…dEaD`) | ⛔ บล็อก |
| ยอดเงินไม่พอจ่ายจำนวนที่โอนรวมค่า gas | ⛔ บล็อก |
| ปลายทางเป็น smart contract | ⚠️ เตือน ต้องติ๊กยืนยันก่อนส่ง |
| ปลายทางเป็น address ใหม่ที่ยังไม่เคยส่งธุรกรรม (อาจพิมพ์ผิดหรือโดน poisoning) | ⚠️ เตือน |
| โอนเกิน 50% ของยอดคงเหลือ | ⚠️ เตือน |
| โอนเข้า address ของตัวเอง | ⚠️ เตือน |

ถ้าแก้ address หรือจำนวนเงินหลังวิเคราะห์แล้ว ระบบจะไม่ให้ส่ง จนกว่าจะให้ U-Bot วิเคราะห์ใหม่ ผู้ใช้จึงเซ็นได้เฉพาะธุรกรรมที่ผ่านการตรวจแล้วเท่านั้น

---

## 🏆 ความสอดคล้องกับเกณฑ์การตัดสิน

### App Execution (40%)
- รันบน Web3 จริงด้วย ethers.js v5 + MetaMask
- สลับไป Avalanche Fuji ให้อัตโนมัติ (`wallet_switchEthereumChain` / `wallet_addEthereumChain`)
- ส่ง native AVAX บน Fuji ได้จริง แสดง tx hash ลิงก์ SnowTrace และสถานะ confirmation
- ใช้ไฟล์ HTML ไฟล์เดียวบน GitHub Pages เปิดได้ทันทีโดยไม่ต้องติดตั้ง

### Practicality (40%)
- แก้ปัญหาการเซ็นหรือโอนผิดพลาด ซึ่งเป็นสาเหตุหลักของการเสียเงินใน Web3
- ใช้ข้อมูล on-chain จริง (bytecode, nonce, balance, gas price) ไม่ใช่ผลลัพธ์ที่เขียนตายตัว
- บล็อกธุรกรรมอันตรายตั้งแต่ก่อนเปิดหน้าต่างเซ็น

### U-Bot Integration (5%)
- U-Bot เป็นผู้ช่วยที่สรุปความเสี่ยงให้อ่านง่าย (✅ / ⚠️ / ⛔) พร้อมคะแนนและคำอธิบาย ก่อนผู้ใช้กดยืนยัน
- เวอร์ชันนี้ใช้ rule engine แบบ on-chain เป็นแกนหลัก และออกแบบให้เพิ่ม LLM / U-Bot API สำหรับถอดรหัส calldata (เช่น `approve`, `setApprovalForAll`) ในเฟสถัดไปได้

---

## 🚀 วิธีทดลองใช้
1. ติดตั้ง [MetaMask](https://metamask.io/)
2. ขอ Fuji AVAX ฟรีจาก [Avalanche Faucet](https://core.app/tools/testnet-faucet/)
3. เปิด Live Demo แล้วกด **Connect Wallet**
4. กรอก address ปลายทางและจำนวนเงิน แล้วกด **🤖 U-Bot AI: Analyze**
   - ลองใส่ `0x0000000000000000000000000000000000000000` เพื่อดูการบล็อก
5. กด **Sign & Send** แล้วเปิดลิงก์ SnowTrace เพื่อตรวจสอบ

## 🧱 Tech Stack
HTML · Tailwind CSS (CDN) · ethers.js 5.7.2 · MetaMask · Avalanche Fuji C-Chain

## 🗺️ Roadmap
- ถอดรหัส calldata ของ ERC-20 `approve` / NFT `setApprovalForAll` และเตือนเมื่อพบ unlimited approval
- เชื่อม U-Bot LLM เพื่ออธิบายธุรกรรมเป็นภาษาคน
- ใช้ blacklist ที่ U-Center DAO ดูแลร่วมกัน (community-reported scam addresses)
- จำลองธุรกรรมก่อนเซ็น (`eth_call` / tracing)

> ⚠️ ใช้กับ Testnet เท่านั้น
