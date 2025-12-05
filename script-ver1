# FOR578 Tình Báo Mối Đe Dọa Mạng
## Phần 4: Phân Tích và Sản Xuất Tình Báo
### Kịch Bản Thuyết Trình

---

## Slide 1: Trang Bìa

**Thời lượng:** 1-2 phút

**Kịch bản:**

Chào mừng đến với Phần 4 của FOR578 - Tình Báo Mối Đe Dọa Mạng. Phần này tập trung vào Phân Tích và Sản Xuất Tình Báo.

Hôm nay chúng ta sẽ cover 4 chủ đề chính:

1. **Case Study: Human-Operated Ransomware** - Hiểu tradecraft của ransomware hiện đại
2. **Exploitation: Storing and Structuring Data** - Lưu trữ threat data với MISP
3. **Analysis: Logical Fallacies and Cognitive Biases** - Nhận diện thiên kiến trong phân tích
4. **Analysis: Exploring Hypotheses** - Phương pháp ACH

Các bài tập thực hành:
- **Exercise 4.1:** Lưu trữ Threat Data trong MISP
- **Exercise 4.2:** Nhận diện Cognitive Biases
- **Exercise 4.3:** Analysis of Competing Hypotheses

---

## Slide 2: Nội Dung Phần

**Thời lượng:** 1-2 phút

**Kịch bản:**

```
┌─────────────────────────────────────────────────────────────────┐
│                    NỘI DUNG PHẦN 4                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1. CASE STUDY: HUMAN-OPERATED RANSOMWARE                       │
│     └── Tradecraft của ransomware hiện đại                      │
│                                                                 │
│  2. EXPLOITATION: STORING AND STRUCTURING DATA                  │
│     ├── Storing Threat Data                                     │
│     ├── Threat Information Sharing                              │
│     └── MISP as a Storage Platform ──────────► Exercise 4.1     │
│                                                                 │
│  3. ANALYSIS: LOGICAL FALLACIES AND COGNITIVE BIASES            │
│     ├── Logical Fallacies                                       │
│     ├── Cognitive Biases                                        │
│     └── Common CTI Informal Fallacies ───────► Exercise 4.2     │
│                                                                 │
│  4. ANALYSIS: EXPLORING HYPOTHESES                              │
│     ├── Analysis of Competing Hypotheses (ACH)                  │
│     ├── Hypotheses Generation                                   │
│     └── Understanding Knowledge Gaps ────────► Exercise 4.3     │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

# PHẦN 1: CASE STUDY - HUMAN-OPERATED RANSOMWARE

---

## Slide 3: Human-Operated Ransomware là gì?

**Thời lượng:** 4-5 phút

**Kịch bản:**

**Định nghĩa:**

Human-operated ransomware khác với ransomware tự động (như WannaCry). Thay vì tự lan truyền qua lỗ hổng, kẻ tấn công thực sự:
- Xâm nhập mạng
- Học môi trường
- Tùy chỉnh cuộc tấn công để đạt tác động tối đa

**Tại sao phổ biến?**

```
┌──────────────────────────────────────────────────────────┐
│  YẾU TỐ HỘI TỤ                                           │
├──────────────────────────────────────────────────────────┤
│  • Nhiều lỗ hổng initial access được phát hiện           │
│  • Công cụ miễn phí: PowerShell, Cobalt Strike           │
│  • Nạn nhân sẵn sàng trả tiền → mô hình kinh doanh       │
│  • Thời gian khắc phục: phút/giờ, không phải ngày        │
└──────────────────────────────────────────────────────────┘
```

**Timeline điển hình:** Khi có domain admin + DC access → triển khai ransomware toàn bộ môi trường gần như ngay lập tức.

---

## Slide 4: Ví Dụ Ransomware Operations

**Thời lượng:** 5-6 phút

**Kịch bản:**

**1. Ryuk Operations:**

```
TrickBot/Emotet → Cobalt Strike → PowerShell (LOTL) → BloodHound → DC → Ryuk
     │                                                                    │
     └────────────────── Khoảng 5 giờ ────────────────────────────────────┘
```

**2. Doppelpaymer:**

```
RDP Access + Dridex → Lateral Movement → Ransomware
         │
         └── Dridex thay vì TrickBot = Activity Group KHÁC
             → Chiến lược điều tra KHÁC
```

**3. Wadhrama:**

```
RDP Brute Force → Credential Theft → Lateral Movement → DC → Ransomware
```

**Điểm chính:** Hiểu tradecraft khác biệt giúp:
- Defenders chuẩn bị playbooks phù hợp
- Incident responders biết cần tìm gì
- Dưới áp lực thời gian khắc nghiệt

---

## Slide 5: "Cái Ác Trông Như Thế Nào"

**Thời lượng:** 3-4 phút

**Kịch bản:**

**Case Study: Tấn công bệnh viện trong COVID-19**

Trong đại dịch COVID-19, có đợt tấn công ransomware có chủ đích nhắm vào bệnh viện.

**Vấn đề:**
- Nhiều SOC trong ngành healthcare không thể operationalize IOC feeds ở quy mô lớn
- Thiếu resources và expertise để xử lý threat intelligence

**Intel hữu ích nhất:**
- Trong tay responders
- Product vendors đã được triển khai

**Bài học quan trọng:**

```
┌─────────────────────────────────────────────────────────────┐
│  ĐIỀU CHỈNH OUTPUTS THEO NĂNG LỰC THỰC TẾ CỦA CONSUMER      │
├─────────────────────────────────────────────────────────────┤
│  • Không phải mọi tổ chức có thể xử lý raw IOC feeds        │
│  • Cung cấp intel ở format phù hợp với khả năng tiếp nhận   │
│  • Prioritize actionable intelligence                       │
└─────────────────────────────────────────────────────────────┘
```

---

# PHẦN 2: EXPLOITATION - STORING AND STRUCTURING DATA

---

## Slide 6: Storing Threat Data - Tổng Quan

**Thời lượng:** 4-5 phút

**Kịch bản:**

**Mục tiêu:** Lưu trữ thông tin ở định dạng truy cập nhanh và hữu ích, có sẵn cho cả security personnel và analysts.

**4 Nguyên tắc cơ bản:**

| Nguyên tắc | Mô tả | Ví dụ |
|------------|-------|-------|
| **Common Format** | Reports và naming thống nhất | Style guides, templates |
| **Scalable** | Lên kế hoạch mở rộng từ đầu | Database architecture |
| **Secure** | Kiểm soát truy cập phù hợp | Role-based permissions |
| **Shareable** | Khả năng chia sẻ với partners | STIX/TAXII support |

**Công cụ lưu trữ:**

```
┌─────────────────────┬─────────────────────────────────────────┐
│  OPEN SOURCE        │  COMMERCIAL                             │
├─────────────────────┼─────────────────────────────────────────┤
│  • MISP             │  • ThreatConnect                        │
│  • Threat_Note      │  • ThreatQuotient                       │
│  • CRITs            │  • Anomali                              │
│                     │  • EclecticIQ                           │
├─────────────────────┼─────────────────────────────────────────┤
│  Ưu: Miễn phí,      │  Ưu: Hỗ trợ đầy đủ, tích hợp sẵn,       │
│      cộng đồng      │       analytics                         │
│  Nhược: Khó deploy  │  Nhược: Chi phí cao                     │
└─────────────────────┴─────────────────────────────────────────┘
```

**Khuyến nghị:** Bắt đầu với open source để hiểu requirements trước khi đầu tư commercial.

---

## Slide 7: Threat Information Sharing

**Thời lượng:** 3-4 phút

**Kịch bản:**

**Tại sao chia sẻ quan trọng?**

- Adversaries chia sẻ tools và techniques
- Defenders cũng cần chia sẻ để bắt kịp
- Collective defense hiệu quả hơn individual defense

**Các mô hình chia sẻ:**

```
┌─────────────────────────────────────────────────────────────┐
│  1. ISAC/ISAO                                               │
│     └── Industry-specific sharing (FS-ISAC, H-ISAC, etc.)   │
│                                                             │
│  2. GOVERNMENT PROGRAMS                                     │
│     └── CISA, FBI InfraGard, etc.                           │
│                                                             │
│  3. PEER-TO-PEER                                            │
│     └── Trusted relationships giữa organizations            │
│                                                             │
│  4. COMMUNITY SHARING                                       │
│     └── MISP communities, Open Threat Exchange              │
└─────────────────────────────────────────────────────────────┘
```

**Standards cho sharing:**

| Standard | Mục đích |
|----------|----------|
| **STIX** | Structured Threat Information eXpression - format dữ liệu |
| **TAXII** | Trusted Automated eXchange - protocol truyền tải |
| **OpenIOC** | Mandiant's IOC format |

---

## Slide 8: MISP as a Storage Platform

**Thời lượng:** 5-6 phút

**Kịch bản:**

**MISP = Malware Information Sharing Platform**

Đã phát triển vượt xa chỉ malware. Được NATO tài trợ, phổ biến trong European CERTs.

**Khả năng chính:**

```
┌─────────────────────────────────────────────────────────────┐
│  MISP CAPABILITIES                                          │
├─────────────────────────────────────────────────────────────┤
│  • IOC management và automation                             │
│  • Role-based user privileges                               │
│  • Tagging và categorization (Galaxies, Taxonomies)         │
│  • Correlation tự động giữa events                          │
│  • ATT&CK framework integration                             │
│  • STIX/TAXII support cho sharing                           │
│  • API cho automation                                       │
└─────────────────────────────────────────────────────────────┘
```

**Cấu trúc MISP:**

```
Organization
    └── Event (= 1 Intrusion/Incident)
            ├── Attributes (IOCs: IPs, domains, hashes, etc.)
            ├── Objects (structured data)
            ├── Tags (ATT&CK, Taxonomies)
            └── Galaxies (threat actors, malware, etc.)
```

**MISP sẽ được sử dụng trong Exercise 4.1**

---

## Slide 9: Exercise 4.1 - Storing Threat Data (MISP)

**Thời lượng:** 3-4 phút

**Kịch bản:**

**Mục tiêu bài tập:**
1. Import indicators vào MISP
2. Tìm correlations giữa intrusions
3. Áp dụng ATT&CK techniques

**Data set: "Leet" Intrusion Set (Edison International)**

```
┌─────────────────────────────────────────────────────────────┐
│  ADVERSARY/CAPABILITY                                       │
│  • Self-extracting RAR files                                │
│  • TildeDrop short names                                    │
│  • Poison Ivy với mutex ")!VoqA.I4"                         │
│  • PlugX                                                    │
│  • Phishing chủ đề hóa đơn (Document2.pdf)                  │
├─────────────────────────────────────────────────────────────┤
│  INFRASTRUCTURE                                             │
│  • Domains: helpdeskforu.net, itsupportnets.com, etc.       │
│  • IPs: 89.34.237.11, 104.224.166.148, 52.74.213.16, etc.   │
├─────────────────────────────────────────────────────────────┤
│  VICTIM                                                     │
│  • Edison International (bids/accounts/financials)          │
│  • Local hosts: 192.168.1.13, 192.168.5.27                  │
└─────────────────────────────────────────────────────────────┘
```

**Credentials MISP VM:** for578@for578.org / SANSForensics!

---

## Slide 10: Exercise 4.1 - Hướng Dẫn Chi Tiết

**Thời lượng:** 5-6 phút

**Kịch bản:**

**Part 1: Import Indicators**

| Bước | Hành động |
|------|-----------|
| 1 | Mở MISP, tạo Event mới cho mỗi Intrusion |
| 2 | Dùng Freetext Import Tool để import IOCs |
| 3 | Review và sửa Category/Type nếu cần |
| 4 | Kiểm tra correlations với existing events |

**Điều chỉnh cần thiết (Intrusion 6):**

| Indicator | Vấn đề | Sửa thành |
|-----------|--------|-----------|
| `alayos(at)supplydenn.com` | Category sai, có "(at)" | Category: Payload delivery, Type: email-src |
| `#V0qA.I4` | Type sai (filename) | Category: Artifacts dropped, Type: mutex |
| `Poison Ivy` | Không được recognize | Manually add: Type: malware-type |

**Part 2: ATT&CK Tagging**

```
Event → Galaxy → ATT&CK → Chọn techniques phù hợp
```

**Câu hỏi phân tích:**
- Technique phổ biến nhất trong dataset là gì?
- Thông tin này giúp defenders như thế nào?

---

## Slide 11: Exercise 4.1 - Kết Quả Mong Đợi

**Thời lượng:** 3-4 phút

**Kịch bản:**

**Correlations tìm được:**

| Intrusion | Related Event | Correlating Value |
|-----------|---------------|-------------------|
| Intrusion 1 | Operation Cloud Hopper | IP `89.34.237.11` |
| Intrusion 3 | Cobalt Strike Beacon + GoGaLocker | IP `89.105.198.28` |
| Intrusion 4 | Operation Cloud Hopper | IP `104.224.166.148` |
| Intrusion 6 | Operation Cloud Hopper | IP `38.72.115.9` |

**ATT&CK Analysis:**

```
Technique phổ biến nhất: T1595 Active Scanning (xuất hiện trong 4 intrusions)

Recommendations cho Defenders:
├── Focus detect Active Scanning
├── Analyze referrers và user-agent strings
├── Monitor suspicious traffic từ single source
└── Detection efforts tập trung vào Initial Access
```

**Lưu ý quan trọng:** Overlaps chỉ cho biết cùng indicators - KHÔNG nên jump to conclusions rằng đây chắc chắn là cùng adversary!

---

# PHẦN 3: ANALYSIS - LOGICAL FALLACIES AND COGNITIVE BIASES

---

## Slide 12: Tại Sao Cần Hiểu Biases?

**Thời lượng:** 3-4 phút

**Kịch bản:**

**Vấn đề cốt lõi:**

Con người không phải máy tính. Chúng ta có xu hướng:
- Tìm evidence confirm điều đã tin
- Ignore evidence contradict
- Đưa ra kết luận quá nhanh

**Trong CTI, điều này có thể dẫn đến:**

```
┌─────────────────────────────────────────────────────────────┐
│  HẬU QUẢ CỦA BIAS TRONG CTI                                 │
├─────────────────────────────────────────────────────────────┤
│  • Attribution sai adversary                                │
│  • Miss các hypotheses thay thế                             │
│  • Overconfidence trong assessments                         │
│  • Recommend phòng thủ không phù hợp                        │
│  • Waste resources vào wrong threats                        │
└─────────────────────────────────────────────────────────────┘
```

**Giải pháp:**
1. Nhận biết các loại fallacies và biases
2. Áp dụng structured analytical techniques (SATs)
3. Peer review và devil's advocacy

---

## Slide 13: Logical Fallacies - Định Nghĩa

**Thời lượng:** 4-5 phút

**Kịch bản:**

**Logical Fallacy = Lỗi trong lập luận logic**

Có 2 loại chính:

```
┌─────────────────────────────────────────────────────────────┐
│  FORMAL FALLACIES                                           │
│  └── Lỗi trong cấu trúc logic của argument                  │
│      Ví dụ: "Nếu A thì B. B đúng. Vậy A đúng." (sai!)       │
├─────────────────────────────────────────────────────────────┤
│  INFORMAL FALLACIES                                         │
│  └── Lỗi trong nội dung hoặc context của argument           │
│      Ví dụ: Appeal to authority, Ad hominem, Straw man      │
└─────────────────────────────────────────────────────────────┘
```

**Ví dụ trong CTI:**

| Fallacy | Ví dụ sai | Vấn đề |
|---------|-----------|--------|
| Affirming the consequent | "APT28 dùng Mimikatz. Attacker này dùng Mimikatz. Vậy đây là APT28." | Nhiều groups dùng Mimikatz |
| Post hoc | "Sau khi install patch, bị compromise. Vậy patch gây ra compromise." | Correlation ≠ Causation |
| False dilemma | "Hoặc là nation-state hoặc script kiddie" | Có nhiều options khác |

---

## Slide 14: Cognitive Biases

**Thời lượng:** 5-6 phút

**Kịch bản:**

**Cognitive Bias = Xu hướng tư duy có hệ thống dẫn đến sai lệch**

Không phải lỗi logic, mà là cách não bộ xử lý thông tin.

**Các biases phổ biến trong CTI:**

```
┌────────────────────┬────────────────────────────────────────┐
│  BIAS              │  MÔ TẢ                                 │
├────────────────────┼────────────────────────────────────────┤
│  Confirmation      │  Tìm evidence confirm điều đã tin       │
│  Anchoring         │  Bám vào thông tin đầu tiên nhận được  │
│  Availability      │  Đánh giá cao things dễ nhớ            │
│  Bandwagon         │  Tin vì nhiều người khác tin           │
│  Hindsight         │  "Tôi biết từ đầu" sau khi biết kết quả│
│  Dunning-Kruger    │  Overestimate khả năng bản thân        │
└────────────────────┴────────────────────────────────────────┘
```

**Ví dụ Anchoring trong CTI:**

```
Reporter: "Tôi set out với ý định show global attacks on ICS..."
           │
           └── Đã có kết luận TRƯỚC khi thu thập data
               → Bias toàn bộ analysis
```

---

## Slide 15: Common CTI Informal Fallacies

**Thời lượng:** 5-6 phút

**Kịch bản:**

**Các fallacies thường gặp trong Cyber Threat Intelligence:**

| Fallacy | Mô tả | Ví dụ CTI |
|---------|-------|-----------|
| **Anecdotal** | Dùng personal experience thay vì data | "Tôi thấy 6000 attacks" (ignore ICS-CERT chỉ report ~200/năm) |
| **Burden of Proof** | Đẩy trách nhiệm chứng minh cho người khác | "ICS bị tấn công daily" (không cung cấp proof) |
| **Congruence** | Chỉ test hypothesis của mình, không test alternatives | Chỉ tìm evidence support APT28, ignore evidence khác |
| **Appeal to Authority** | Tin vì expert nói | "FireEye nói vậy nên đúng" |
| **Hasty Generalization** | Kết luận từ sample nhỏ | "2 samples có Chinese characters → Chinese APT" |

**Cách phòng tránh:**

```
┌─────────────────────────────────────────────────────────────┐
│  1. Luôn hỏi: "Evidence nào BÁC BỎ hypothesis này?"         │
│  2. Tìm alternative explanations                            │
│  3. Quantify uncertainty (dùng estimative language)         │
│  4. Peer review với người có góc nhìn khác                  │
│  5. Dùng structured techniques như ACH                      │
└─────────────────────────────────────────────────────────────┘
```

---

## Slide 16: Exercise 4.2 - Identifying Cognitive Biases

**Thời lượng:** 3-4 phút

**Kịch bản:**

**Mục tiêu:** Đọc bài báo ICS honeypot và identify ít nhất 2 cognitive biases/logical fallacies.

**Bài báo:** "A Decoy Computer Was Set Up Online" (ICS Honeypot)

**Các biases có thể tìm thấy:**

| Bias/Fallacy | Vị trí trong bài | Evidence |
|--------------|------------------|----------|
| **Anchoring** | Đầu bài | "set out with the idea of showing global attacks on ICSs" |
| **Anecdotal** | Claims về số lượng | "6,000 attacks from U.S." (ignore ICS-CERT data) |
| **Burden of Proof** | Claims về threats | "ICS under daily assault" (không có proof) |
| **Congruence** | Xuyên suốt | Chỉ tìm evidence support hypothesis |
| **Unsubstantiated Claims** | Nhiều nơi | Claims không có data backup |

**Takeaway:** Identifying biases trong reports của người khác giúp recognize biases trong chính mình.

---

## Slide 17: Exercise 4.2 - Phân Tích Chi Tiết

**Thời lượng:** 4-5 phút

**Kịch bản:**

**Phân tích bias trong bài báo:**

**1. Anchoring Bias ⚓**
```
Reporter: "set out with the idea of showing..."
          │
          ├── Đã có kết luận TRƯỚC khi thu thập data
          ├── Thay vì hỏi "Are there attacks?" → assume có và chứng minh
          └── Cách đúng: Approach với open mind, để data quyết định
```

**2. Anecdotal Fallacy 📊**
```
Reporter claims: 6,000 attacks from U.S., 3,500 from China...
                 │
                 ├── Dựa hoàn toàn vào data từ honeypots cá nhân
                 ├── Ignore: ICS-CERT chỉ report ~200 incidents/năm
                 └── Prioritize personal data over wider evidence
```

**3. Burden of Proof ⚖️**
```
Claim: "ICS networks being under daily assault..."
       │
       ├── Không cung cấp proof cho statement
       └── Đẩy burden cho reader phải chứng minh reporter sai
```

---

# PHẦN 4: ANALYSIS - EXPLORING HYPOTHESES

---

## Slide 18: Analysis of Competing Hypotheses (ACH)

**Thời lượng:** 5-6 phút

**Kịch bản:**

**ACH là gì?**

Phương pháp luận để giảm bias trong phân tích, phát triển bởi Richards Heuer Jr. (CIA, 45 năm kinh nghiệm).

**Ý tưởng cốt lõi:**

```
Thay vì: Tìm evidence để CONFIRM điều đã tin
         ↓
Làm:     Đánh giá có hệ thống NHIỀU hypotheses với TẤT CẢ evidence
```

**Khi nào dùng ACH?**

| Dùng ACH khi... | Không cần ACH khi... |
|-----------------|----------------------|
| High stakes decisions | Routine analysis |
| Multiple plausible explanations | Clear-cut cases |
| Significant uncertainty | Abundant evidence |
| Potential for bias | Low-impact assessments |

---

## Slide 19: ACH - Quy Trình 7 Bước

**Thời lượng:** 6-8 phút

**Kịch bản:**

```
┌─────────────────────────────────────────────────────────────┐
│                    QUY TRÌNH ACH                            │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  BƯỚC 1: HYPOTHESES                                         │
│  └── Liệt kê TẤT CẢ hypotheses có thể, kể cả ít khả thi     │
│                                                             │
│  BƯỚC 2: EVIDENCE                                           │
│  └── Thu thập evidence cho MỖI hypothesis (cả ủng hộ & bác) │
│                                                             │
│  BƯỚC 3: DIAGNOSTICS (Ma trận)                              │
│  └── Tạo matrix: Hypotheses (hàng) × Evidence (cột)         │
│      Đánh dấu: + (support), - (refute), 0 (neutral)         │
│                                                             │
│  BƯỚC 4: REFINEMENT                                         │
│  └── Loại evidence không diagnostic (support tất cả đều)    │
│                                                             │
│  BƯỚC 5: PRIORITIZATION                                     │
│  └── Xếp hạng hypotheses - TÌM EVIDENCE BÁC BỎ TRƯỚC        │
│                                                             │
│  BƯỚC 6: SENSITIVITY                                        │
│  └── Kết luận phụ thuộc vào evidence nào? Nếu sai thì sao?  │
│                                                             │
│  BƯỚC 7: CONCLUSION                                         │
│  └── Báo cáo với estimative language và confidence level    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

**Nguyên tắc quan trọng:** Bắt đầu bằng LOẠI BỎ options thay vì CONFIRM preferred hypothesis.

---

## Slide 20: Hypotheses Generation

**Thời lượng:** 4-5 phút

**Kịch bản:**

**Nguyên tắc tạo hypotheses:**

| Nguyên tắc | Mô tả |
|------------|-------|
| **Giải thích evidence** | Hypothesis phải explain available evidence |
| **Brainstorm với người khác** | Góc nhìn đa dạng bắt blind spots |
| **Đừng xét khả thi ban đầu** | Bao gồm cả options ít có khả năng |
| **Phân biệt chưa chứng minh vs đã bác bỏ** | Chỉ loại khi có evidence sai |

**Chưa chứng minh vs Đã bác bỏ:**

```
┌─────────────────────────────────────────────────────────────┐
│  CHƯA CHỨNG MINH (Unproven)                                 │
│  └── Không có evidence nó đúng                              │
│      → VẪN GIỮ trong analysis                               │
├─────────────────────────────────────────────────────────────┤
│  ĐÃ BÁC BỎ (Disproven)                                      │
│  └── Có evidence tích cực nó SAI                            │
│      → LOẠI khỏi analysis                                   │
└─────────────────────────────────────────────────────────────┘
```

**Lưu ý:** Chỉ loại hypothesis khi có evidence chúng sai, KHÔNG phải vì thiếu evidence chúng đúng.

---

## Slide 21: Understanding and Identifying Knowledge Gaps

**Thời lượng:** 4-5 phút

**Kịch bản:**

**Quan trọng ngang việc biết: Hiểu điều bạn KHÔNG biết.**

**Các loại knowledge gaps:**

```
┌─────────────────────────────────────────────────────────────┐
│  1. MISSING EVIDENCE                                        │
│     └── Evidence mong đợi nhưng không thấy. Tại sao vắng?   │
│                                                             │
│  2. COLLECTION LIMITATIONS                                  │
│     └── Không thể quan sát gì? Blind spots về visibility?   │
│                                                             │
│  3. ASSUMPTIONS                                             │
│     └── Đang coi gì là hiển nhiên mà không có bằng chứng?   │
│                                                             │
│  4. INTELLIGENCE REQUIREMENTS                               │
│     └── Cần thêm thông tin gì để reach higher confidence?   │
└─────────────────────────────────────────────────────────────┘
```

**Trong báo cáo, phải ghi rõ:**
- Confidence level (Low/Medium/High)
- Cơ sở cho confidence level
- Knowledge gaps ảnh hưởng assessment
- Evidence nào nếu thay đổi sẽ thay đổi conclusion

---

## Slide 22: Exercise 4.3 - ACH (Money Bin Case)

**Thời lượng:** 4-5 phút

**Kịch bản:**

**Scenario:**
- Money Bin bị ransomware
- IR team kết luận: ransomware là diversion để hide exfiltration
- CTI team được yêu cầu review conclusion này

**Step 1: Formulate Hypotheses**

| # | Hypothesis |
|---|------------|
| H1 | Ransomware là diversion (để cover exfiltration) |
| H2 | Có 2 separate intrusions, ransomware KHÔNG phải diversion |
| H3 | Không có intrusion, là false positive |
| H4 | Intrusion thứ 2 là insider threat |

**Step 2: List Evidence**

| # | Evidence |
|---|----------|
| E1 | Exfiltration xảy ra TRƯỚC ransomware (18h trước outbreak) |
| E2 | Không tìm thấy other intrusions trong network |
| E3 | TrickBot được tìm thấy gần temporal proximity với exfil |
| E4 | Outside firm đã handle và confirm ransomware case |
| E5 | Không có public cases của TrickBot/Ryuk dùng làm diversion |
| E6 | "Robin Hood" theming không overlap với TrickBot intrusion |
| E7 | Ryuk operations thường nhanh; delay tạo window cho exfil |
| E8 | Money Bin là financial organization |

---

## Slide 23: Exercise 4.3 - ACH Matrix

**Thời lượng:** 5-6 phút

**Kịch bản:**

**Step 3: Map Evidence to Hypotheses**

| Evidence | H1 (Diversion) | H2 (Separate) | H3 (False +) | H4 (Insider) |
|----------|----------------|---------------|--------------|--------------|
| E1: Exfil trước ransomware | + | - | - | - |
| E2: Không có other intrusions | + | - | + | + |
| E3: TrickBot gần temporal với exfil | + | - | - | - |
| E4: Outside firm confirm ransomware | 0 | 0 | - | 0 |
| E5: Không có public diversion cases | - | + | 0 | 0 |
| E6: Robin Hood không overlap TrickBot | - | + | 0 | 0 |
| E7: Ryuk nhanh, delay tạo window | + | - | 0 | 0 |
| E8: Money Bin là financial org | 0 | 0 | 0 | 0 |

**Legend:** + = supports, - = refutes, 0 = neutral

**Step 4: Refinement**

E8 (Money Bin là financial org) → Non-diagnostic, loại khỏi analysis.

---

## Slide 24: Exercise 4.3 - Prioritization & Conclusion

**Thời lượng:** 5-6 phút

**Kịch bản:**

**Step 5: Prioritize Hypotheses**

| Hypothesis | Refuting (-) | Supporting (+) | Neutral (0) |
|------------|--------------|----------------|-------------|
| **H1** | 2 | 4 | 2 |
| H4 | 2 | 1 | 5 |
| H3 | 3 | 1 | 4 |
| H2 | 4 | 2 | 2 |

**Ranking:** H1 > H4 > H3 > H2 (H1 ít refuting nhất, nhiều supporting nhất)

**Step 6: Sensitivity Analysis**

Critical evidence: E2 ("Không có other intrusions")
→ NẾU evidence này bị refuted, assessment sẽ thay đổi significantly.

**Step 7: Final Report**

```
┌─────────────────────────────────────────────────────────────┐
│  ASSESSMENT                                                 │
├─────────────────────────────────────────────────────────────┤
│  CTI team assesses với LOW CONFIDENCE rằng ransomware       │
│  operation là diversion để hide exfiltration.               │
│                                                             │
│  Basis: Limited time để complete analysis.                  │
│                                                             │
│  Key evidence: Không tìm thấy other intrusions trong        │
│  network. Nếu thay đổi → cần reassess.                      │
│                                                             │
│  Intelligence gap: Chưa identify public cases TrickBot/     │
│  Ryuk dùng làm diversion.                                   │
└─────────────────────────────────────────────────────────────┘
```

---

## Slide 25: Bài Học Chính

**Thời lượng:** 4-5 phút

**Kịch bản:**

**Tóm tắt các bài học quan trọng:**

```
┌─────────────────────────────────────────────────────────────┐
│  1. HUMAN-OPERATED RANSOMWARE                               │
│     • Map TTPs với defender playbooks                       │
│     • Điều chỉnh outputs theo năng lực consumer             │
├─────────────────────────────────────────────────────────────┤
│  2. STORING THREAT DATA (MISP)                              │
│     • Common format, scalable, secure, shareable            │
│     • Correlation giúp identify overlaps                    │
│     • ATT&CK tagging giúp prioritize defenses               │
├─────────────────────────────────────────────────────────────┤
│  3. COGNITIVE BIASES                                        │
│     • Nhận biết biases trong reports của người khác         │
│     • Giúp recognize biases trong chính mình                │
│     • Structured techniques giảm thiểu impact               │
├─────────────────────────────────────────────────────────────┤
│  4. ACH                                                     │
│     • Tìm evidence BÁC BỎ trước                             │
│     • Quantify uncertainty với estimative language          │
│     • Ghi rõ knowledge gaps và evidentiary dependence       │
└─────────────────────────────────────────────────────────────┘
```

**Quote:** *"Here is my lens. You know my methods."* - Sherlock Holmes

Phương pháp phân tích - analytical tradecraft - là điều phân biệt phân tích tốt với xấu.

---

## Phụ Lục: Tóm Tắt Thời Gian

| Slide | Chủ đề | Thời lượng |
|-------|--------|------------|
| 1 | Trang bìa | 1-2 phút |
| 2 | Nội dung phần | 1-2 phút |
| 3 | Human-Operated Ransomware là gì? | 4-5 phút |
| 4 | Ví dụ Ransomware Operations | 5-6 phút |
| 5 | "Cái Ác Trông Như Thế Nào" | 3-4 phút |
| 6 | Storing Threat Data - Tổng Quan | 4-5 phút |
| 7 | Threat Information Sharing | 3-4 phút |
| 8 | MISP as a Storage Platform | 5-6 phút |
| 9 | Exercise 4.1 - Overview | 3-4 phút |
| 10 | Exercise 4.1 - Hướng dẫn | 5-6 phút |
| 11 | Exercise 4.1 - Kết quả | 3-4 phút |
| 12 | Tại sao cần hiểu Biases? | 3-4 phút |
| 13 | Logical Fallacies | 4-5 phút |
| 14 | Cognitive Biases | 5-6 phút |
| 15 | Common CTI Informal Fallacies | 5-6 phút |
| 16 | Exercise 4.2 - Overview | 3-4 phút |
| 17 | Exercise 4.2 - Phân tích | 4-5 phút |
| 18 | ACH là gì? | 5-6 phút |
| 19 | ACH - Quy trình 7 bước | 6-8 phút |
| 20 | Hypotheses Generation | 4-5 phút |
| 21 | Knowledge Gaps | 4-5 phút |
| 22 | Exercise 4.3 - Scenario & Hypotheses | 4-5 phút |
| 23 | Exercise 4.3 - ACH Matrix | 5-6 phút |
| 24 | Exercise 4.3 - Conclusion | 5-6 phút |
| 25 | Bài học chính | 4-5 phút |
| **Tổng** | | **~100-120 phút** |

---

## Ghi Chú cho Người Thuyết Trình

1. **Tương tác:** Khi thảo luận biases, hỏi audience họ đã thấy những điều này trong công việc chưa.

2. **Điểm dừng:** Sau MISP (Slide 8), sau Biases (Slide 17), và trước ACH (Slide 18).

3. **Demo MISP:** Nếu có thể, show live demo MISP interface khi cover Exercise 4.1.

4. **ACH Practice:** Có thể làm ACH exercise như group activity với audience tham gia brainstorm hypotheses.

5. **Real-world examples:** Gắn ví dụ với incidents gần đây mà audience có thể biết.

---

> 📚 **Tham khảo:** Xem file `CTI_Section4_Glossary_VN.md` để có thông tin kỹ thuật chi tiết về các khái niệm được đề cập.
