# FOR578 Tình Báo Mối Đe Dọa Mạng
## Phần 4: Phân Tích và Sản Xuất Tình Báo
### Kịch Bản Thuyết Trình

---

## Slide 1: Trang Bìa

**Thời lượng:** 1-2 phút

**Kịch bản:**

Chào mừng đến với Phần 4 của FOR578 - Tình Báo Mối Đe Dọa Mạng. Phần này tập trung vào Phân Tích và Sản Xuất Tình Báo.

Trong các phần trước, chúng ta đã đề cập:
- Phần 1: Tình Báo Mối Đe Dọa Mạng và Yêu Cầu
- Phần 2: Bộ Kỹ Năng Cơ Bản - Phân Tích Xâm Nhập
- Phần 3: Các Nguồn Thu Thập

Hôm nay, chúng ta sẽ đi sâu vào cách phân tích dữ liệu đã thu thập và sản xuất tình báo có thể hành động. Đây là nơi công việc phân tích thực sự diễn ra - nơi chúng ta biến đổi dữ liệu thô thành insights có thể bảo vệ tổ chức của chúng ta.

Phần này được phát triển bởi Robert M. Lee và Rebekah Brown, dựa trên kinh nghiệm sâu rộng của họ trong cả hoạt động tình báo chính phủ và tình báo mối đe dọa khu vực tư nhân.

---

## Slide 2: Nội Dung Phần

**Thời lượng:** 2-3 phút

**Kịch bản:**

Luồng nội dung hôm nay theo dàn ý của sách:
- **Case Study: Human-Operated Ransomware** — tập trung tradecraft, bao gồm đợt tấn công bệnh viện ("Cái ác trông như thế nào").
- **Defender-Focused Outputs** — làm tình báo dễ tiêu thụ với hình ảnh trực quan và phòng thủ ưu tiên.
- **Exploitation: Lưu trữ và Cấu trúc Dữ liệu** — nền tảng, best practices, và data set "Leet" dẫn vào Bài tập 4.1.
- **Analysis: Các Loại Phân Tích Khác Nhau** — link, visual, data/temporal/trend analysis, plus case Panama Papers và Bài tập 4.4.
- **Analysis: Clustering Intrusions** — style guides, naming/rosetta stone, cảnh báo về báo cáo bên ngoài, meta-features, ACH cho clusters, và Rule of 2 với Bài tập 4.5.
- **Analysis: Logical Fallacies và Cognitive Biases** — trở ngại cho lập luận đúng đắn.
- **Analysis: Khám phá Giả thuyết** — các bước ACH, tạo giả thuyết, và lỗ hổng kiến thức.

Chúng ta sẽ kết nối mỗi chủ đề với các bài tập để team có thể thực hành tradecraft.

---

## Slide 3: Human-Operated Ransomware

**Thời lượng:** 6-8 phút

**Kịch bản:**

Ransomware đã tồn tại hàng thập kỷ, nhưng human-operated ransomware (mã độc tống tiền do con người điều khiển) đại diện cho một bước tiến hóa đáng kể trong bối cảnh mối đe dọa.

**Điều gì làm nó khác biệt?**

Không giống ransomware tự lan truyền như WannaCry - lan truyền tự động qua các lỗ hổng - human-operated ransomware liên quan đến kẻ tấn công thực sự xâm nhập mạng của bạn, học môi trường của bạn, và tùy chỉnh cuộc tấn công để đạt tác động tối đa.

**Tại sao điều này trở nên phổ biến?**

Nhiều yếu tố hội tụ:
- Nhiều lỗ hổng initial access hơn đang được phát hiện và khai thác
- Các công cụ như PowerShell và Cobalt Strike có sẵn miễn phí
- Quan trọng nhất - nạn nhân sẵn sàng trả tiền. Điều này tạo ra mô hình kinh doanh có lợi nhuận.

Thời gian để khắc phục thường được đo bằng phút hoặc giờ, không phải ngày. Khi kẻ tấn công có domain admin credentials và quyền truy cập vào domain controller, họ có thể triển khai ransomware trên toàn bộ môi trường của bạn gần như ngay lập tức.

**Hãy xem ba ví dụ:**

**Ryuk Operations:** Kill chain điển hình bắt đầu với TrickBot hoặc Emotet làm initial access. Kẻ tấn công sau đó triển khai Cobalt Strike, sử dụng PowerShell cho kỹ thuật living-off-the-land, chạy BloodHound để map Active Directory, chiếm Domain Controller, và triển khai Ryuk. Toàn bộ quy trình này có thể xảy ra trong khoảng 5 giờ.

**Doppelpaymer:** Ở đây chúng ta thấy RDP access kết hợp với Dridex malware. Điều này quan trọng - nếu bạn thấy Dridex thay vì TrickBot, bạn đang đối mặt với Activity Group khác, có nghĩa là chiến lược điều tra khác.

**Wadhrama:** Loại này thường bắt đầu với RDP brute forcing, tiếp theo là credential theft, lateral movement, và triển khai ransomware qua DC.

**"Cái ác trông như thế nào":** Trong COVID-19 có đợt tấn công có chủ đích nhắm vào bệnh viện. Nhiều SOC trong ngành đó không thể operationalize IOC feeds ở quy mô lớn. Intel hữu ích nhất nằm trong tay responders và product vendors đã được triển khai. Bài học: điều chỉnh outputs theo năng lực thực tế của consumer.

**Điểm chính cho CTI analysts:** Hiểu sự khác biệt tradecraft giữa các operations này giúp defenders chuẩn bị playbooks phù hợp và giúp incident responders biết cần tìm gì, dưới áp lực thời gian khắc nghiệt.

> 📚 **Tham khảo chi tiết:** Xem file `CTI_Section4_Glossary_VN.md` để có thông tin kỹ thuật sâu về TrickBot, Emotet, Cobalt Strike, BloodHound, LOTL, Dridex khi bị hỏi.

---

## Slide 4: Defender-Focused Outputs

**Thời lượng:** 3-4 phút

**Kịch bản:**

- Sử dụng timelines/kill-chain visuals với MITRE ATT&CK overlays để làm tradecraft dễ tiêu thụ.
- Ghép mỗi tactic với 1-2 biện pháp phòng thủ hàng đầu; tránh danh sách dài làm overwhelm responders.
- Tuyệt vời cho tabletop exercises và đào tạo non-CTI audiences; tập trung vào kết quả defenders có thể hành động ngay.

---

## Slide 5: Lưu Trữ và Cấu Trúc Dữ Liệu

**Thời lượng:** 5-6 phút

**Kịch bản:**

Bây giờ hãy nói về thách thức thực tế mà mọi CTI team đều gặp: làm thế nào bạn lưu trữ và cấu trúc threat data của mình?

Mục tiêu đơn giản về khái niệm nhưng thách thức trong thực hiện: lưu trữ thông tin ở định dạng truy cập nhanh và hữu ích, có sẵn cho cả security personnel và analysts sẽ sản xuất thông tin.

**Bạn có hai loại công cụ chính:**

**Open Source options** bao gồm MISP, Threat_Note, và CRITs.

Ưu điểm rõ ràng - miễn phí, lưu trữ đầy đủ, và kết nối với cộng đồng chia sẻ open-source. Nhược điểm? Có thể khó triển khai và bảo trì. Bạn cần chuyên môn kỹ thuật để deploy và vận hành hiệu quả.

**Commercial options** bao gồm ThreatConnect, ThreatQuotient, Anomali, và EclecticIQ.

Những nền tảng này được hỗ trợ đầy đủ với dễ cài đặt, tích hợp sẵn, và khả năng data analytics. Đánh đổi là chi phí - những nền tảng này có thể đắt, và có thể không phù hợp workflows đã thiết lập của bạn.

**Khuyến nghị của tôi:** Bắt đầu với open source. Sử dụng nó để hiểu requirements của bạn. Tìm hiểu bạn thực sự cần gì trước khi đầu tư vào commercial tools. Khi team mở rộng, bạn có thể đánh giá commercial options với hiểu biết rõ ràng về những tính năng quan trọng với bạn.

**Dù chọn nền tảng nào, hãy tuân theo các best practices này:**

1. **Common Format** - Đảm bảo personnel lưu reports ở định dạng chung. Điều này bao gồm cả reports và naming conventions.
2. **Scalable** - Nhu cầu lưu trữ sẽ tăng. Lên kế hoạch mở rộng.
3. **Secure** - Threat intelligence reports là mục tiêu có giá trị. Triển khai kiểm soát truy cập và xác thực phù hợp.
4. **Shareable** - Ngay cả khi bạn không chia sẻ bên ngoài bây giờ, hãy xây dựng khả năng. Cuối cùng bạn sẽ muốn chia sẻ với partners.

Chúng ta sẽ sử dụng các nguyên tắc này trực tiếp trong Bài tập 4.1.

---

## Slide 6: Bài Tập 4.1 Dẫn Vào — Lưu Trữ Threat Data (Leet Set)

**Thời lượng:** 2-3 phút

**Kịch bản:**

Chúng ta sẽ làm việc với một tập con của **"Leet" intrusion set**:
- **Adversary/Capability:** self-extracting RAR files, TildeDrop short names, Poison Ivy với `)!VoqA.I4`, PlugX, phishing chủ đề hóa đơn (`Document2.pdf`).
- **Infrastructure:** phân phối qua `helpdeskforu[.]net`, `itsupportnets[.]com`, `itservicesinc[.]net`, `supportdesk[.]biz`, plus follow-on IPs (61.221.40.61, 76.29.149.73/.78/.133, 68.72.115.39, 89.34.237.11, 104.224.166.148, 52.74.213.16).
- **Victim:** Hệ thống Edison International liên quan đến bids/accounts/financials; local hosts 192.168.1.13 và 192.168.5.27; personas 133757AF, 1337d00d, fjerk.

Nhiệm vụ: lưu trữ dữ liệu này ở định dạng chung, bảo mật, có thể chia sẻ để nó vẫn hữu ích sau khi analysts thay đổi.

---

## Slide 7: MISP - Malware Information Sharing Platform

**Thời lượng:** 5-6 phút

**Kịch bản:**

Hãy xem xét kỹ hơn MISP, một trong những nền tảng threat intelligence open-source được sử dụng rộng rãi nhất.

MISP là viết tắt của Malware Information Sharing Platform, mặc dù nó đã phát triển vượt xa malware. Nó được NATO tài trợ và có sự chấp nhận đặc biệt mạnh mẽ trong các European CERTs.

**Khả năng Chính:**
- **Tập trung vào IOCs và automation** - Đây là công cụ yêu thích của analysts. MISP được thiết kế để quản lý indicator hiệu quả.
- **Role-based user privileges** - Bạn có thể kiểm soát ai thấy gì và ai có thể làm gì.
- **Full logging và traceability** - Mọi hành động được theo dõi. Điều này quan trọng cho accountability.
- **RESTful API và job scheduling** - Automation được tích hợp từ đầu.

**Export Formats:**

MISP có thể export sang STIX XML, JSON, CSV, IDS rules, và tích hợp trực tiếp với các nền tảng SIEM chính. Sự linh hoạt này có nghĩa bạn có thể lấy dữ liệu ra ở bất kỳ định dạng nào tools của bạn cần.

**Import Sources:**

Bạn có thể import từ ThreatConnect, OpenIOC, và thậm chí trích xuất indicators từ PDFs. Điều này giúp dễ dàng hơn để consolidate intelligence từ nhiều nguồn.

**Tạo Event:**

Workflow đơn giản:
1. Thêm Event với metadata
2. Thêm files - malware samples, phishing emails, v.v.
3. Thêm Attributes - các indicators thực tế

**Visual Correlation:**

Một trong những tính năng mạnh mẽ của MISP là visual indicator correlation. Khi bạn thêm events theo thời gian, MISP có thể hiển thị connections giữa events dựa trên shared indicators. Điều này tiết lộ patterns mà bạn có thể bỏ lỡ khi xem individual events riêng lẻ.

Ví dụ, bạn có thể phát hiện ba cuộc xâm nhập riêng biệt trong sáu tháng đều sử dụng cùng domain registrant email address. Đó là connection cho bạn biết điều gì đó về adversary.

---

## Slide 8: Các Loại Phân Tích Khác Nhau

**Thời lượng:** 4-5 phút

**Kịch bản:**

Analysts phản ứng khác nhau với các loại phân tích khác nhau. Biết điều gì hiệu quả cho bạn và team:
- **Know Thyself (Hiểu bản thân):** Format nào kích hoạt tư duy của BẠN? Text? Graphs? Tables?
- **Know the Team (Hiểu team):** Ghép cặp người có preferences bổ trợ; chọn tools phù hợp với họ.
- **Inject New Approaches:** Với vấn đề quan trọng, ép bản thân thử phương pháp thứ hai để không bị anchoring vào một lăng kính.

Chúng ta sẽ áp dụng nhiều loại—link, visual, data/temporal, và trend analysis—trong phần này.

---

## Slide 9: Link Analysis & Tools

**Thời lượng:** 5-6 phút

**Kịch bản:**

Link analysis map các mối quan hệ giữa entities. Để hữu ích:
- **Sources:** Mỗi entity cần provenance (nguồn gốc).
- **Context:** Mỗi link cần "how/when" (VD: domain resolved to IP vào ngày/giờ).

Common tools: Maltego/CaseFile, Analyst's Notebook, Palantir, Centrifuge, Gephi/Graphviz, Neo4J, Titan, Linkurious, Cambridge Intelligence (KeyLines).

Sử dụng dữ liệu đã normalize; inputs phi cấu trúc cần preprocessing (VD: parsing IOCs từ văn bản).

---

## Slide 10: Data, Temporal, và Trend Analysis

**Thời lượng:** 4-5 phút

**Kịch bản:**

- **Data analysis:** Clean/transform/model để phát hiện correlations mới; data science và ML có thể bổ trợ CTI.
- **Temporal analysis:** Coi thời gian là biến độc lập; timelines có thể phát hiện clustering (VD: spike trong 445 scans trước WannaCry).
- **Trend analysis:** Hoàn thành Kill Chains/Diamond Models theo thời gian và tích hợp external intel để thấy patterns tạm thời vs lâu dài.

Các phương pháp này thường đi trước hoặc làm giàu link/visual analysis.

---

## Slide 11: Case Study — Panama Papers (Link Analysis Quy Mô Lớn)

**Thời lượng:** 4-5 phút

**Kịch bản:**

- 2.6 TB, 11.5M files (emails, PDFs, images), 107 tổ chức báo chí, 80 quốc gia, 25 ngôn ngữ.
- Tech stack: Apache Solr + Tika cho indexing, Nuix cho OCR, Neo4J + Linkurious cho link analysis.
- Ví dụ: Mạng lưới Sergey Roldugin—shell companies, dòng tiền, người hưởng lợi.
- **Thách thức của dữ liệu:** Họ bắt đầu với requirements, sau đó chọn tools. Đừng để tooling đi trước mục tiêu phân tích.

Tại sao quan trọng cho CTI: cùng kỹ năng/tools áp dụng cho large intrusion datasets. Bắt đầu với requirements trước tools.

---

## Slide 12: Visual Analysis trong Maltego (Bài Tập 4.4)

**Thời lượng:** 3-4 phút

**Kịch bản:**

- Biểu diễn trực quan tăng tốc phát hiện pattern; bubble views đánh trọng số entities xuất hiện nhiều.
- Pivoting trong UI có thể tiết lộ relationships tinh tế bị bỏ qua trong tables.
- Bài tập 4.4: lấy Excel "first cut," sau đó dùng Maltego để visualize và pivot. Mong đợi chi phí chuẩn bị cao hơn nhưng insights phong phú hơn.

Giữ sources và context hiển thị trong graph.

---

## Slide 13: Tổng Quan Clustering Intrusions

**Thời lượng:** 5-6 phút

**Kịch bản:**

Clustering gắn intrusions vào sets/campaigns hỗ trợ requirements—không chỉ đuổi theo actors.
- Dùng nhiều lăng kính (Kill Chain, Diamond Model) để tìm overlaps.
- ACH giúp khi có ambiguity hoặc analysts bất đồng.
- Luôn có bucket "other intrusion set" để tránh ép dữ liệu.

Clustering tốn công; chúng ta cũng sẽ đề cập shortcuts (Rule of 2) cho triage.

---

## Slide 14: Style Guide và Quy Ước Đặt Tên

**Thời lượng:** 4-5 phút

**Kịch bản:**

Thiết lập **Style Guide** để clustering có thể lặp lại:
- Cấu trúc team, lexicon được chấp nhận, từ cần tránh.
- Mẫu structured analytic techniques và intelligence requirements.
- Hướng dẫn tạo clusters/products và key processes cần tuân theo.

**Naming:** 
- Nên: dùng tên lấy cảm hứng từ incident, tối nghĩa, hoặc hài hước.
- Không nên: đặt theo tools/TTPs hoặc dùng numbered/attribution-based schemes; tools được dùng chung và enumerations nhập nhằng.
- Rủi ro của clever schemes (VD: động vật cho quốc gia): lỗi attribution khóa bạn vào label sai. Giảm thiểu: cho phép rename và tránh hard-coding attribution trong tên.
- MITRE ATT&CK "Associated Groups" và Rosetta Stone nội bộ giúp dịch vendor names; track references, đừng merge mù quáng các vendor-defined clusters.

---

## Slide 15: APT Rosetta Stone & Báo Cáo Bên Ngoài

**Thời lượng:** 4 phút

**Kịch bản:**

- **APT Groups and Operations Matrix:** Duy trì Rosetta Stone map public/vendor names sang internal group names của bạn. Hữu ích cho giao tiếp nhưng đừng merge analytical models.
- **External Intrusion Reports:** Đánh giá cao observable data hơn kết luận của vendor. Giữ definitions và criteria riêng của bạn; tránh dùng vendor names cho campaigns của bạn để giữ quyền kiểm soát narrative và evidence base.

---

## Slide 16: Diamond Model Meta-Features

**Thời lượng:** 3-4 phút

**Kịch bản:**

Diamond core features: Adversary, Capability (TTPs), Infrastructure, Victim. Meta-features thêm độ chặt chẽ:
- Timestamp (start/end), phase, result, direction, methodology, resources.
- Gán confidence cho mỗi feature/meta-feature (ít nhất High/Moderate/Low).

Dùng meta-features khi correlating intrusions và đánh giá confidence.

---

## Slide 17: ACH cho Cluster Correlation

**Thời lượng:** 4-5 phút

**Kịch bản:**

Khi mapping intrusions vào clusters/campaigns:
- Dùng ACH khi evidence thưa thớt, intrusions map vào nhiều clusters, hoặc analysts bất đồng.
- Phân loại evidence theo Kill Chain và Diamond Model features; ghi nhận confidence cho mỗi feature/meta-feature.
- Luôn có "other campaign" để tránh false certainty; external reports có thể inform nhưng giữ definitions riêng.

---

## Slide 18: Khi Nào Dùng ACH cho Quyết Định Intrusion-Cluster

**Thời lượng:** 2-3 phút

**Kịch bản:**

ACH cho clustering hữu ích nhất khi:
- Correlation mơ hồ do evidence hạn chế.
- Một intrusion map hợp lý vào nhiều clusters được định nghĩa tương tự.
- Analysts bất đồng về correlation.

Nếu correlation rõ ràng và không có tranh cãi, ACH có thể là overkill; nếu không, dùng nó để định lượng uncertainty.

---

## Slide 19: Shortcut — Rule of 2 (Bài Tập 4.5)

**Thời lượng:** 4-5 phút

**Kịch bản:**

**Rule of 2** cluster intrusions khi **hai đỉnh Diamond overlap nhất quán** (VD: Infrastructure + Capability, hoặc Adversary + Infrastructure).
- Tìm đặc điểm độc đáo: IPs đại học Trung Quốc cụ thể + Poison Ivy mutex ⇒ cluster (VD: FUZZYSQUIRREL).
- Xoay features khi cần để sàng lọc lượng lớn nhanh chóng; sau đó bước lại để validate với requirements.
- Focus Victim + Capability có thể tạo defensive playbooks mà nhiều adversaries có thể fit.

Bài tập 4.5: áp dụng cả formal definition và Rule of 2 để xây dựng activity groups từ intrusions được cung cấp.

---

## Slide 20: Vòng Đời Campaign — Khi Nào Retire Clusters

**Thời lượng:** 3-4 phút

**Kịch bản:**

- Trạng thái campaign: active, inactive, dormant.
- Giữ thông tin cluster vô thời hạn; intrusions tương lai có thể làm sáng tỏ clusters quá khứ.
- Định nghĩa lại clusters khi có evidence mới; clusters tương lai có thể correlate với clusters quá khứ. Chỉ retire khi có lý do phân tích, không chỉ vì activity im lặng.

---

## Slide 21: Logical Fallacies và Cognitive Biases

**Thời lượng:** 6-8 phút

**Kịch bản:**

Bây giờ chúng ta vào phần tôi coi là quan trọng nhất cho mọi analyst: hiểu các trở ngại cho phân tích chính xác.

**Để nói thẳng: Tất cả analysts đều có bias.**

Nơi bạn sinh ra, quan điểm chính trị, quan điểm tôn giáo, lương, quốc tịch - tất cả những điều này định hình cách bạn nhìn thế giới. Bạn không thể loại bỏ bias. Nhưng bạn có thể hiểu nó và làm việc để giảm thiểu tác động của nó.

**Có một sự phân biệt quan trọng tôi muốn bạn nhớ:**

Security personnel - incident responders, SOC analysts - thường được trả tiền cho kinh nghiệm. Khi responder thấy một pattern nhất định, kinh nghiệm giúp họ nhanh chóng xác định điều gì đang xảy ra. Điều đó có giá trị.

**Tuy nhiên, CTI analysts được trả tiền để đánh bại bias của mình.** Chúng ta phải nhận ra khi kinh nghiệm đang giúp chúng ta vs khi nó đang dẫn chúng ta đi lạc.

**Logical Fallacies** là lỗi trong lập luận. Chúng xảy ra khi arguments không logic.

**Anecdotal Fallacy (Ngụy biện giai thoại):** "Tôi đã làm incident như thế này trước và đó là Trung Quốc, nên phân tích của bạn rằng không phải chắc chắn sai." Kinh nghiệm cá nhân ghi đè evidence thuyết phục.

**Appeal to Probability (Kêu gọi xác suất):** "Nga đang hack mọi thứ dạo này, nên intrusion có lẽ là Nga." Chỉ vì điều gì đó có thể xảy ra không làm nó thành kết luận logic. Chúng ta cần phân tích evidence.

**Burden of Proof (Gánh nặng chứng minh):** "Chứng minh họ không làm đi." Điều này đảo ngược luồng logic đúng. Người đưa ra tuyên bố phải cung cấp evidence.

**Cognitive Biases** là ràng buộc về cách chúng ta nghĩ ảnh hưởng đến quyết định sai.

**Mirror Imaging (Tưởng tượng gương):** Phân tích tình huống dùng ngữ cảnh của bạn thay vì của adversary. "Nếu tôi là tình báo Nga..." - dừng ngay đó. Bạn không phải, và quá trình suy nghĩ của bạn sẽ không khớp với họ.

**Confirmation Bias (Thiên kiến xác nhận):** Tìm evidence hỗ trợ giả thuyết ưa thích trong khi bác bỏ hoặc giảm nhẹ evidence mâu thuẫn.

**Anchoring (Neo giữ):** Đánh giá quá cao thông tin đầu tiên nhận được. Thông tin sớm "neo" phân tích của bạn, khiến khó điều chỉnh ngay cả khi có thông tin mới.

**Congruence Bias (Thiên kiến phù hợp):** Chỉ test giả thuyết ưa thích thay vì tạo alternatives.

**Correlation vs. Causation:** "Cum hoc ergo propter hoc" — correlation không phải causation; đừng suy ra nguyên nhân từ sự đồng thời.

---

## Slide 22: Ngụy Biện Phi Hình Thức Thường Gặp trong CTI

**Thời lượng:** 5-6 phút

**Kịch bản:**

Hãy xem một số ngụy biện phi hình thức xuất hiện thường xuyên trong CTI assessments. Đây là arguments mà kết luận không theo từ tiền đề.

**Appeal to the Stone (Kêu gọi đá):** Bác bỏ tuyên bố là vô lý mà không cung cấp bằng chứng nào.

"Nghĩ Mỹ sẽ xâm phạm chính phủ đồng minh là vô lý. Tiếp đi."

Điều này nguy hiểm vì nó đóng phân tích mà không thực sự bác bỏ gì. Đôi khi sự thật LÀ đáng ngạc nhiên.

**Argument from Silence (Lập luận từ im lặng):** Chấp nhận kết luận vì không có evidence chống lại.

"Tôi có bằng chứng không phải UK và không có bằng chứng không phải Đức. Vậy, tôi đánh giá là Đức."

Sự thiếu vắng evidence không phải evidence. Ngụy biện này đặc biệt phổ biến khi intelligence không đầy đủ.

**Argument from Repetition (Lập luận từ lặp lại):** Tranh luận lâu đến mức mọi người chấp nhận kết luận chỉ để kết thúc thảo luận.

"Chúng ta ở đây năm tiếng rồi; thôi, Iran đã làm cuộc tấn công."

Mệt mỏi họp không bao giờ nên quyết định kết luận phân tích.

**Middle Ground (Điểm giữa):** Giả định thỏa hiệp giữa hai vị trí phải là sự thật.

"Bạn tin đó là chính phủ Nga và tôi tin đó là tội phạm mạng Nga, vậy cả hai đồng ý đó là Nga."

Đôi khi một vị trí đơn giản là đúng và cái kia sai. Chia đôi sự khác biệt không tạo ra độ chính xác.

**Illusory Correlation (Tương quan ảo):** Quan sát correlation khi không tồn tại.

Case study kinh điển: 8 tháng 7, 2015. NYSE dừng giao dịch do lỗi máy tính. Tất cả chuyến bay United Airlines bị grounded. Website Wall Street Journal down. Nhiều người giả định một cuộc tấn công mạng phối hợp. Thực tế? Ba sự cố kỹ thuật không liên quan xảy ra cùng ngày.

**Hindsight Bias (Thiên kiến hồi cố):** "Tôi biết từ đầu rồi."

Sau incident, dễ tin kết quả là rõ ràng và có thể dự đoán. Điều này dẫn đến đổ lỗi nạn nhân và đơn giản hóa quá mức sự phức tạp của phòng thủ. Nation-state APT intrusions thường khó dự đoán - thiên kiến hồi cố đơn giản hóa không phù hợp thực tế đó.

---

## Slide 23: Analysis of Competing Hypotheses (ACH)

**Thời lượng:** 8-10 phút

**Kịch bản:**

Bây giờ hãy thảo luận về phương pháp luận mạnh mẽ để giảm bias trong phân tích: Analysis of Competing Hypotheses, hay ACH.

Đây được phát triển bởi Richards Heuer Jr., cựu binh 45 năm của CIA. Cuốn sách "Psychology of Intelligence Analysis" của ông nên là bắt buộc đọc cho mọi CTI analyst.

**Ý tưởng cốt lõi:** Thay vì tìm evidence để xác nhận điều bạn đã tin, bạn đánh giá có hệ thống nhiều giả thuyết với tất cả evidence có sẵn.

**Quy Trình 7 Bước:**

**Bước 1: Hypothesis** - Liệt kê tất cả giả thuyết có thể. Chưa xét khả thi. Bao gồm options ít có khả năng. Brainstorm với người khác để có góc nhìn đa dạng.

**Bước 2: Evidence** - Với mỗi giả thuyết, tìm cả evidence ủng hộ VÀ bác bỏ. Bao gồm assumptions và suy luận cũng như hard evidence. Ghi nhận evidence mong đợi nhưng thiếu.

**Bước 3: Diagnostics** - Xây ma trận so sánh. Liệt kê giả thuyết qua hàng trên, evidence xuống cột bên. Với mỗi evidence, đánh dấu nó ủng hộ, bác bỏ, hay trung lập cho mỗi giả thuyết.

**Bước 4: Refinement** - Loại bỏ evidence không giúp phân biệt giữa giả thuyết. Nếu evidence hỗ trợ bằng nhau tất cả options, nó không diagnostic. Điều này cũng có thể tiết lộ giả thuyết mới bạn chưa xét.

**Bước 5: Prioritization** - Bây giờ xếp hạng giả thuyết. Đây là insight quan trọng: **Tìm evidence BÁC BỎ giả thuyết trước.** Bắt đầu bằng loại bỏ options thay vì xác nhận ưa thích.

**Bước 6: Sensitivity** - Kết luận phụ thuộc vào lượng nhỏ evidence đến mức nào? Nếu evidence đó sai hoặc thay đổi thì sao? Ghi nhận những rủi ro này.

**Bước 7: Conclusion** - Báo cáo findings. Bao gồm giả thuyết đã xét, evidence chính, và định lượng đúng đắn assessment dùng ngôn ngữ rõ ràng.

**Nhìn vào ma trận ví dụ:**

Chúng ta có ba giả thuyết - H1, H2, H3. Chúng ta đánh giá evidence như "Chinese IP C2," "Poison Ivy RAT," và "Defense Sector targeting."

Bạn có thể thấy H1 có nhiều supporting evidence nhất. Nhưng quan trọng hơn, H2 và H3 có contradicting evidence giúp loại bỏ chúng.

**Nguyên tắc chính:** Kết luận phân tích nên luôn được coi là tạm thời. Khi evidence mới xuất hiện, sẵn sàng revisit phân tích.

---

## Slide 24: Tạo Giả Thuyết & Lỗ Hổng Kiến Thức

**Thời lượng:** 5-6 phút

**Kịch bản:**

Trước khi áp dụng ACH, chúng ta cần tạo giả thuyết tốt. Hãy để tôi chia sẻ một số nguyên tắc để làm điều này hiệu quả.

**Tạo Giả Thuyết:**

**Giải thích tất cả evidence** - nhưng nhớ, không phải mọi giả thuyết cần giải thích mọi evidence. Đôi khi evidence không fit gọn gàng.

**Brainstorm với người khác** - góc nhìn đa dạng bắt blind spots. Bao gồm người có backgrounds và expertise khác nhau.

**Ban đầu đừng xét khả thi** - bao gồm options ít có khả năng. Bạn có thể loại bỏ sau nếu evidence không hỗ trợ.

**Bao gồm giả thuyết chưa chứng minh, loại trừ đã bác bỏ** - đây là phân biệt quan trọng.
- Chưa chứng minh: Không có evidence đúng
- Đã bác bỏ: Có evidence tích cực nó sai

Chỉ loại giả thuyết khi có evidence chúng sai, không chỉ vì thiếu evidence chúng đúng.

**Xác Định Lỗ Hổng Kiến Thức:**

Quan trọng ngang là hiểu điều bạn KHÔNG biết.

**Missing Evidence:** Evidence nào bạn mong đợi thấy nhưng không? Tại sao nó có thể vắng?

**Collection Limitations:** Bạn không thể quan sát gì? Đâu là blind spots về visibility?

**Assumptions:** Bạn đang coi gì là hiển nhiên mà không có bằng chứng?

**Intelligence-Driven Hypothesis Generation:**

Sự kiện lớn tạo cơ hội cho adversaries - và cho analysts. Khi khủng hoảng xảy ra, threat groups hoạt động với nation-state motivation thường không thể chuẩn bị đầy đủ. Họ mắc lỗi OpSec. Họ reuse tradecraft.

Case study Panama Papers là ví dụ tuyệt vời. Nhà báo trên toàn thế giới dùng link analysis trên massive dataset để tiết lộ patterns tham nhũng. Cho CTI, chúng ta nên nghĩ: sự kiện lớn ảnh hưởng nation-states sẽ có khả năng tạo targeted operations. Theo dõi threat groups pivot để target các tổ chức liên quan.

**Nhớ:** Uncertainty càng lớn hoặc impact của kết luận càng cao, bạn nên xét nhiều giả thuyết thay thế hơn.

---

## Slide 25: Bài Học Chính

**Thời lượng:** 4-5 phút

**Kịch bản:**

Hãy tóm tắt các bài học quan trọng từ phần này:

1) **Human-operated ransomware cần hiểu tradecraft.** Map TTPs với defender playbooks; điều chỉnh outputs theo năng lực consumer thực tế (VD: bệnh viện).
2) **Làm intel dễ tiêu thụ.** Dùng visuals và phòng thủ ưu tiên để hỗ trợ responders.
3) **Lưu intel ở định dạng chung, bảo mật, scalable, shareable.** Áp dụng trong Bài tập 4.1; MISP là một option, không phải duy nhất.
4) **Dùng nhiều loại phân tích.** Link/visual/data/temporal/trend analysis plus case-driven examples (Panama Papers) tiết lộ patterns vượt xa IOCs.
5) **Cluster có kỷ luật.** Style guides, naming cẩn thận, Rosetta Stone tracking, thận trọng báo cáo bên ngoài, meta-features, ACH cho ambiguity, và Rule of 2 shortcut giữ clusters defensible.
6) **Quản lý lifecycle.** Giữ lịch sử cluster; định nghĩa lại/retire dựa trên evidence, không phải im lặng.
7) **Nhận ra và đánh bại bias.** Logical fallacies và cognitive biases là hằng số; ACH và alternative hypothesis generation giảm thiểu chúng.

Khi quan trọng, đừng tin trực giác một mình—xây ma trận, tìm evidence bác bỏ trước, và rõ ràng về uncertainty.

---

Tôi sẽ kết thúc với quote từ tài liệu khóa học, được gán cho Sherlock Holmes:

**"Here is my lens. You know my methods."** (Đây là ống kính của tôi. Bạn biết phương pháp của tôi.)

Là threat intelligence analysts, phương pháp của chúng ta - analytical tradecraft của chúng ta - là điều phân biệt phân tích tốt với xấu. Làm chủ những phương pháp này. Áp dụng chúng nhất quán. Và luôn khiêm tốn về điều bạn không biết.

Cảm ơn. Có câu hỏi không?

---

## Phụ Lục: Tóm Tắt Thời Gian

| Slide | Chủ đề | Thời lượng |
|-------|--------|------------|
| 1 | Trang bìa | 1-2 phút |
| 2 | Nội dung phần | 2-3 phút |
| 3 | Human-Operated Ransomware | 6-8 phút |
| 4 | Defender-Focused Outputs | 3-4 phút |
| 5 | Lưu trữ và Cấu trúc Dữ liệu | 5-6 phút |
| 6 | Bài tập 4.1 Dẫn vào (Leet Set) | 2-3 phút |
| 7 | MISP Platform | 5-6 phút |
| 8 | Các Loại Phân Tích | 4-5 phút |
| 9 | Link Analysis & Tools | 5-6 phút |
| 10 | Data/Temporal/Trend Analysis | 4-5 phút |
| 11 | Case Study: Panama Papers | 4-5 phút |
| 12 | Visual Analysis trong Maltego (BT 4.4) | 3-4 phút |
| 13 | Tổng quan Clustering Intrusions | 5-6 phút |
| 14 | Style Guide & Naming | 4-5 phút |
| 15 | APT Rosetta Stone & External Reports | 4 phút |
| 16 | Diamond Model Meta-Features | 3-4 phút |
| 17 | ACH cho Cluster Correlation | 4-5 phút |
| 18 | Khi Nào Dùng ACH (Clusters) | 2-3 phút |
| 19 | Rule of 2 (BT 4.5) | 4-5 phút |
| 20 | Campaign Lifecycle | 3-4 phút |
| 21 | Logical Fallacies và Cognitive Biases | 6-8 phút |
| 22 | Ngụy biện Phi hình thức CTI | 5-6 phút |
| 23 | Analysis of Competing Hypotheses | 8-10 phút |
| 24 | Tạo Giả thuyết & Lỗ hổng Kiến thức | 5-6 phút |
| 25 | Bài học Chính | 4-5 phút |
| **Tổng** | | **101-140 phút** |

---

## Ghi Chú cho Người Thuyết Trình

1. **Tương tác với ví dụ** - Khi thảo luận fallacies và biases, hỏi audience họ đã thấy những điều này trong công việc của mình chưa.
2. **Dừng cho câu hỏi** - Điểm dừng tự nhiên sau MISP (Slide 7), sau khối phương pháp phân tích (Slide 12), và trước bias/ACH (Slides 21/23).
3. **Tham chiếu bài tập** - Bài tập 4.1 (Lưu trữ Threat Data), Bài tập 4.4 (Visual Analysis trong Maltego), Bài tập 4.5 (Rule of 2 clustering), plus bài tập bias/ACH (4.2/4.3) để giữ lớp hands-on.
4. **Kết nối với phần trước** - Dùng Kill Chain và Diamond Model xuyên suốt thảo luận clustering và ACH.
5. **Currency thực tế** - Gắn ví dụ ransomware với incidents gần đây; cho clustering/naming, đề cập naming collisions của vendor hiện tại audience sẽ nhận ra.
