Tôi sẽ giải chi tiết 3 bài tập cho bạn:

---

# 📝 GIẢI BÀI TẬP FOR578 SECTION 4

---

## Exercise 4.1: Storing Threat Data and Information (MISP)

### Part 1: Indicators

**Câu 1: Có phải update các events đã nhập trước đó không?**

> **Trả lời:** Có, phải update attributes cho Intrusion 1 và 2.

**Giải thích:** Khi review Intrusion 1, thấy IP `89.34.237.11` có Category là "Payload delivery" nhưng theo Style Guide phải là "Network activity". Cần sửa lại.

---

**Câu 2: Khi dùng Freetext Import Tool cho Intrusion 6, cần điều chỉnh gì?**

> **Trả lời:** Phải thay đổi Type của email address và mutex, đồng thời manually add attribute cho Poison Ivy.

**Chi tiết điều chỉnh:**

| Indicator | Vấn đề | Sửa thành |
|-----------|--------|-----------|
| `alayos(at)supplydenn.com` | Category sai, có "(at)" | Category: Payload delivery, Type: email-src, thay "(at)" → "@" |
| `#V0qA.I4` | Type sai (filename) | Category: Artifacts dropped, Type: mutex |
| `Poison Ivy` | Không được recognize | Manually add: Category: Payload installation, Type: malware-type |

---

**Câu 3: Có intrusions nào match với external reports không?**

> **Trả lời:** Có, Intrusions 1, 3, 4, và 6 đều correlate với previous events.

**Correlations tìm được:**

| Intrusion | Related Event | Correlating Value |
|-----------|---------------|-------------------|
| Intrusion 1 | Operation Cloud Hopper | IP `89.34.237.11` |
| Intrusion 3 | Cobalt Strike Beacon Tweet + GoGaLocker/MegaCortex | IP `89.105.198.28` |
| Intrusion 4 | Operation Cloud Hopper | IP `104.224.166.148` |
| Intrusion 6 | Operation Cloud Hopper | IP `38.72.115.9` |

---

**Câu 4: Những overlaps này cho biết điều gì? Có thể form hypotheses gì?**

> **Trả lời:** Overlaps chỉ cho biết cùng indicators xuất hiện trong intrusions của ta và previous reporting. **KHÔNG nên jump to conclusions** rằng đây "chắc chắn" là cùng một adversary.

**Hypotheses có thể form:**
- Different adversaries sử dụng cùng infrastructure (phổ biến trong RaaS)
- Intrusion 3 có thể involve Cobalt Strike, IP `89.105.198.28` là C2 server được reuse bởi multiple ransomware operators
- Cần further investigation và gather thêm evidence

---

### Part 2: ATT&CK Techniques

**Câu 6: Technique phổ biến nhất trong dataset?**

> **Trả lời:** T1595 Active Scanning (ATT&CK v8) hoặc T1254 Conduct active scanning (ATT&CK v7)

**Cách tìm:** Global Actions → Statistics → Galaxy Matrix → Xem heat map, technique có màu đỏ (value = 4) là phổ biến nhất.

---

**Câu 7: Tại sao thông tin này relevant? Có thể recommend gì cho defenders?**

> **Trả lời:** 

**Relevance:**
- Cho phép prioritize techniques dựa trên internal data thực tế
- Team không còn overwhelmed với quá nhiều techniques trong ATT&CK

**Recommendations cho Defenders:**
- Focus vào detect Active Scanning vì đã observe trong 4 intrusions
- Analyze referrers và user-agent strings
- Monitor suspicious network traffic từ single source
- Detection efforts tập trung vào Initial Access stage

---

## Exercise 4.2: Identifying Cognitive Biases

**Task:** Đọc bài báo "A Decoy Computer Was Set Up Online" và identify ít nhất 2 cognitive biases/logical fallacies.

### Các Biases/Fallacies Tìm Được:

#### 1. Anchoring Bias ⚓

**Vị trí:** Đầu bài báo

**Evidence:** Reporter nói rằng ông ta "set out with the idea of showing the global nature of attacks against ICSs"

**Vấn đề:** 
- Đã có kết luận sẵn trước khi thu thập data
- Thay vì hỏi "Are there global attacks against ICSs?" → đã assume là có và tìm cách chứng minh

**Cách đúng:** Nên approach với open mind, để data quyết định conclusion.

---

#### 2. Anecdotal Fallacy 📊

**Vị trí:** Phần claims về số lượng attacks

**Evidence:** 
- Reporter claim "6,000 attacks from U.S., 3,500 from China, 2,500 from Russia"
- Dựa hoàn toàn vào data từ honeypots của họ

**Vấn đề:**
- Ignore community data: ICS-CERT chỉ report ~200 incidents/năm
- Chỉ có handful of incidents thực sự qualify là "attack"
- Prioritize data cá nhân thu thập over wider evidence

---

#### 3. Burden of Proof Fallacy ⚖️

**Vị trí:** Claims về ICS threats

**Evidence:** "ICS networks being under daily assault by hackers, and that threat is only growing..."

**Vấn đề:**
- Không cung cấp proof cho statement
- Đẩy burden of proof cho reader phải chứng minh reporter sai

---

#### 4. Congruence Bias 🔍

**Vị trí:** Xuyên suốt bài báo

**Evidence:** Reporter chỉ tìm evidence support hypothesis của mình

**Vấn đề:**
- Không consider alternative hypotheses
- Không test xem data có thể explain bằng cách khác không

---

#### 5. Unsubstantiated Claims

**Ví dụ các claims không có proof:**

| Claim | Vấn đề |
|-------|--------|
| "the data largely reflect reconnaissance missions, in which hackers often use less obfuscation" | Assumption không có visibility vào available data |
| "the U.S. is the conduit for a lot of the world's attack traffic" | Không có evidence support |

---

### Key Takeaways Exercise 4.2:

1. Identifying biases trong reports của người khác giúp recognize biases trong team và bản thân
2. Cognitive biases phổ biến trong everyday life, kể cả news reports
3. Combination của technical field (ICS) + CTI tạo nhiều opportunities cho mistakes

---

## Exercise 4.3: Analysis of Competing Hypotheses (ACH)

### Scenario Recap:
- Money Bin bị ransomware
- IR team kết luận: ransomware là diversion để hide exfiltration
- CTI team được yêu cầu review conclusion này

### Step 1: Formulate Hypotheses

| # | Hypothesis |
|---|------------|
| H1 | Ransomware là diversion (để cover exfiltration) |
| H2 | Có 2 separate intrusions, ransomware KHÔNG phải diversion |
| H3 | Không có intrusion, là false positive |
| H4 | Intrusion thứ 2 là insider threat |

---

### Step 2: List Evidence

| # | Evidence |
|---|----------|
| E1 | Exfiltration xảy ra TRƯỚC ransomware (18h trước outbreak, 10h sau TrickBot) |
| E2 | Không tìm thấy other intrusions trong network |
| E3 | TrickBot được tìm thấy gần temporal proximity với exfiltration |
| E4 | Outside firm đã handle và confirm ransomware case |
| E5 | Không có public cases của TrickBot/Ryuk được dùng làm diversion |
| E6 | "Robin Hood" theming không overlap với TrickBot intrusion |
| E7 | Ryuk operations thường diễn ra nhanh; delay tạo window cho exfiltration |
| E8 | Money Bin là financial organization |

---

### Step 3: Map Evidence to Hypotheses

| Evidence | H1 | H2 | H3 | H4 |
|----------|----|----|----|----|
| E1: Exfil trước ransomware | + | - | - | - |
| E2: Không có other intrusions | + | - | + | + |
| E3: TrickBot gần temporal với exfil | + | - | - | - |
| E4: Outside firm confirm ransomware | 0 | 0 | - | 0 |
| E5: Không có public TrickBot/Ryuk diversion cases | - | + | 0 | 0 |
| E6: Robin Hood không overlap TrickBot | - | + | 0 | 0 |
| E7: Ryuk nhanh, delay tạo window | + | - | 0 | 0 |
| E8: Money Bin là financial org | 0 | 0 | 0 | 0 |

**Legend:** + = supports, - = refutes, 0 = neutral

---

### Step 4: Refine Matrix

**Non-diagnostic evidence:** E8 (Money Bin là financial org) - không impact hypothesis nào → loại khỏi analysis.

---

### Step 5: Prioritize Hypotheses

| Hypothesis | Refuting (-) | Supporting (+) | Neutral (0) |
|------------|--------------|----------------|-------------|
| **H1** | 2 | 4 | 2 |
| H4 | 2 | 1 | 5 |
| H3 | 3 | 1 | 4 |
| H2 | 4 | 2 | 2 |

**Ranking (highest → lowest likelihood):**

```
🥇 H1: Ransomware là diversion (ít refuting nhất, nhiều supporting nhất)
🥈 H4: Insider threat  
🥉 H3: False positive
4️⃣ H2: Two separate intrusions (nhiều refuting nhất)
```

---

### Step 6: Evidentiary Dependence

**Critical evidence:**
- **E1 & E3:** Temporal proximity - rất useful, similar nature
- **E2:** "Không có other intrusions" - NẾU evidence này bị refuted, assessment sẽ thay đổi significantly

---

### Step 7: Final Report

> **Assessment:**
> 
> CTI team **không có lý do để refute** assessment của IR team. CTI team **assesses với LOW CONFIDENCE** rằng ransomware operation là diversion để hide exfiltration of data related to high net worth individuals.
>
> **Basis for low confidence:** Limited time để complete analysis.
>
> **Key evidence:** Không tìm thấy other intrusions trong network. Nếu evidence này thay đổi, CTI team cần reassess conclusion.
>
> **Alternative hypotheses considered:**
> - Two separate intrusions (ransomware không phải diversion)
> - False positive (không có intrusion)
> - Second intrusion là insider threat
>
> **Non-diagnostic evidence:** Money Bin là financial organization không help prioritize hypotheses.
>
> **Key intelligence gap:** Team chưa identify public cases của TrickBot/Ryuk được dùng làm diversion. Identifying cases như vậy sẽ help reach higher confidence.

---

### Step 8: Identify Milestones

**Milestones có thể impact assessment:**

1. **Forensics cho thấy multiple intrusions** tại thời điểm exfiltration → cần re-analyze
2. **Public report** về TrickBot/Ryuk được dùng làm diversion → strengthen H1
3. **Insider threat indicators** được discover → strengthen H4
4. **Brazilian IP** được link tới known threat actor → new direction

---

### Key Takeaways Exercise 4.3:

1. **ACH giúp hedge against confirmation bias** bằng cách force compare evidence với multiple hypotheses
2. **Không cho "right answer"** - chỉ giúp structure thinking
3. **Support IR team's conclusion** với analysis backup
4. **Important for decisions** có significant implications cho company

---

Bạn cần tôi giải thích thêm phần nào hoặc tạo file tổng hợp không?
