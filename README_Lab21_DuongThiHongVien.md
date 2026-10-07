# Lab 21 — Phân tích rủi ro AI qua case study thực tế

- **Họ và tên:** Dương Thị Hồng Viên
- **MSSV / mã học viên:** 2A202602385
- **Lớp:** Track 1
- **Ngành đã chọn:** Y tế — AI hỗ trợ quyết định lâm sàng (clinical decision support)

> Phạm vi: Tôi xem xét AI và thuật toán dự đoán được dùng để hỗ trợ phân bổ chăm sóc, cảnh báo nguy cơ và phân loại tổn thương da. Các mức rủi ro dưới đây là nhận định định tính cho bài tập, không phải kết luận pháp lý. Nguồn nghiên cứu thường đo hiệu năng trên dữ liệu hồi cứu hoặc bộ kiểm thử; khi nguồn không chứng minh tổn hại lâm sàng đã xảy ra, tôi ghi đó là nguy cơ, không phải sự kiện.

## 1. Industry Risk Snapshot

| Nội dung | Đánh giá của tôi và lý do |
| --- | --- |
| Những tác hại chính có thể xảy ra | Bỏ sót hoặc chậm chẩn đoán; phân loại sai mức khẩn cấp; phân bổ hỗ trợ chăm sóc không công bằng; cảnh báo sai gây mệt mỏi cảnh báo; lộ dữ liệu sức khỏe. Người bệnh chịu rủi ro sức khỏe trực tiếp, còn bác sĩ và cơ sở y tế có thể chịu áp lực thời gian, trách nhiệm và giảm niềm tin. |
| Mức độ high-stakes | **Cao.** Đầu ra có thể ảnh hưởng việc người bệnh được ưu tiên, xét nghiệm, điều trị hay chuyển khám lúc nào. Sai sót trong tình huống cấp cứu hoặc ung thư có thể dẫn tới hậu quả nghiêm trọng. AI chỉ nên hỗ trợ; không tự quyết định thay nhân viên y tế. |
| Dữ liệu nhạy cảm có thể được sử dụng | Hồ sơ bệnh án, triệu chứng, thuốc, xét nghiệm, ảnh y khoa, tiền sử, tuổi, giới tính, chủng tộc/dân tộc và dữ liệu thanh toán/bảo hiểm. Tôi không sử dụng dữ liệu cá nhân thật trong bài. |
| Nhu cầu human review | **Cao.** Bác sĩ/điều dưỡng cần xem xét kết quả, bối cảnh lâm sàng và dấu hiệu bất thường trước quyết định; quy trình phải cho phép bỏ qua hoặc sửa cảnh báo, ghi nhận lý do, chuyển tuyến và kiểm toán sai lệch theo nhóm bệnh nhân. Cần rà soát định kỳ hiệu năng sau triển khai. |

## 2. Case study 1 — Thuật toán quản lý chăm sóc dựa trên chi phí dự đoán (Obermeyer et al., 2019)

### Brief Case

- **Tổ chức / sản phẩm AI:** Một thuật toán thương mại được dùng rộng rãi trong quản lý sức khỏe quần thể để xác định bệnh nhân có nguy cơ cao cần hỗ trợ chăm sóc bổ sung. Nhóm nghiên cứu không nêu tên nhà cung cấp trong bài báo.
- **Thời gian, địa điểm / bối cảnh:** Nghiên cứu tại Hoa Kỳ, công bố năm 2019; đánh giá thuật toán đang được dùng trong bối cảnh hệ thống y tế thương mại.
- **AI được dùng để làm gì:** Xếp hạng nguy cơ để chọn bệnh nhân tham gia chương trình quản lý chăm sóc. Biến đích của mô hình là **chi phí y tế dự kiến**, được dùng làm đại diện (proxy) cho nhu cầu sức khỏe.
- **Vấn đề hoặc sự kiện đáng chú ý:** Với cùng điểm nguy cơ, bệnh nhân da đen có tình trạng bệnh nặng hơn bệnh nhân da trắng. Do chi phí chăm sóc phản ánh cả chênh lệch tiếp cận dịch vụ, dự đoán chi phí thấp có thể khiến bệnh nhân da đen ít được đưa vào nhóm nhận trợ giúp hơn dù nhu cầu sức khỏe cao.
- **Số liệu có nguồn:** Nếu sửa cách chọn để dự đoán nhu cầu bệnh thay vì chi phí, tỷ lệ bệnh nhân da đen được xác định để nhận trợ giúp bổ sung trong phân tích tăng từ **17,7% lên 46,5%**. Đây là kết quả phân tích của nhóm nghiên cứu, không phải tỷ lệ chẩn đoán sai trong mọi bệnh viện.
- **Nguồn:** [Obermeyer, Powers, Vogeli & Mullainathan, “Dissecting racial bias in an algorithm used to manage the health of populations,” Science 366(6464), 447–453 (25/10/2019), doi:10.1126/science.aax2342](https://doi.org/10.1126/science.aax2342); tóm tắt và thông tin trích dẫn tại [PubMed](https://pubmed.ncbi.nlm.nih.gov/31649194/).
- **Phân biệt bằng chứng và nhận định:** Bằng chứng nghiên cứu xác nhận sai lệch phân bổ theo chủng tộc và chỉ ra proxy chi phí là một nguyên nhân. Nhận định của tôi là trong triển khai tương tự, việc không kiểm tra phân bổ có thể làm chậm tiếp cận chương trình quản lý bệnh cho nhóm vốn ít được chăm sóc. Nghiên cứu không chứng minh một hậu quả sức khỏe cụ thể đã xảy ra với từng bệnh nhân.

### Harm Map Worksheet

| Trường | Phân tích của tôi |
| --- | --- |
| High-risk moment | Khi điểm nguy cơ được dùng để chọn ai được mời vào chương trình hỗ trợ chăm sóc hoặc được ưu tiên theo dõi. |
| Stakeholder bị ảnh hưởng | Bệnh nhân da đen và các nhóm có chi phí chăm sóc thấp hơn do rào cản tiếp cận; bác sĩ, nhân viên quản lý ca và cơ sở y tế sử dụng danh sách xếp hạng. |
| Failure mode | **Proxy/label bias:** mô hình dự đoán chi phí thay cho mức độ bệnh hoặc nhu cầu chăm sóc; dữ liệu lịch sử mang theo chênh lệch tiếp cận điều trị. |
| Layer bắt đầu lỗi | **Model / mục tiêu dự đoán.** Bài báo truy nguyên sai lệch tới việc chọn chi phí làm proxy. Dữ liệu xã hội tạo bối cảnh, nhưng lỗi thiết kế trực tiếp nằm ở mục tiêu mà mô hình tối ưu. |
| Harm xảy ra là gì? | **Đã được chứng minh:** bệnh nhân da đen có mức độ bệnh nặng hơn tại cùng điểm nguy cơ và được chọn ít hơn để nhận hỗ trợ. **Nguy cơ:** một số người có nhu cầu có thể bị bỏ khỏi chương trình, dẫn tới hỗ trợ muộn hoặc thiếu; bài báo không đo trực tiếp kết cục sức khỏe sau đó. |
| Harm lens | Công bằng và phân biệt đối xử; sức khỏe; quyền tiếp cận dịch vụ chăm sóc. |
| Severity | **High.** Quyết định ảnh hưởng khả năng tiếp cận hỗ trợ y tế; mức độ nghiêm trọng cụ thể phụ thuộc bệnh trạng và chương trình. |
| Scale | **High trong hệ thống dùng thuật toán:** bài báo mô tả cách tiếp cận tương tự ảnh hưởng hàng triệu bệnh nhân; con số 17,7% → 46,5% là kết quả của phân tích nghiên cứu, không ngoại suy thành số người bị hại thực tế. |
| Probability | **High đối với sai lệch phân bổ trong hệ thống được nghiên cứu**, vì chênh lệch đã quan sát được. Xác suất một bệnh nhân cụ thể bị tổn hại sức khỏe chưa được nghiên cứu định lượng. |
| Frequency | Có thể lặp lại mỗi lần hệ thống xếp hạng bệnh nhân; tần suất quyết định cụ thể tại từng cơ sở không được nguồn công bố. |
| Vì sao? | Chênh lệch 17,7%/46,5% và cơ chế proxy được báo cáo trong nghiên cứu Science. Nhận định về hỗ trợ muộn là nguy cơ có cơ sở, không phải kết cục đã đo. Cần đánh giá theo nhóm, thay proxy bằng mục tiêu sức khỏe phù hợp và cho nhân viên y tế quyền rà soát danh sách. |

## 3. Case study 2 — Epic Sepsis Model dự đoán nguy cơ nhiễm khuẩn huyết (Wong et al., 2021)

### Brief Case

- **Tổ chức / sản phẩm AI:** Epic Sepsis Model (ESM), mô hình độc quyền của Epic Systems tích hợp trong hồ sơ sức khỏe điện tử, được thiết kế tạo cảnh báo khi bệnh nhân có thể sắp bị nhiễm khuẩn huyết (sepsis).
- **Thời gian, địa điểm / bối cảnh:** Nghiên cứu hồi cứu tại Michigan Medicine, Đại học Michigan, Hoa Kỳ; dữ liệu nhập viện từ 06/12/2018 đến 20/10/2019. Bài báo công bố trực tuyến ngày 21/06/2021.
- **AI được dùng để làm gì:** Tính điểm dự đoán định kỳ từ dữ liệu hồ sơ điện tử nhằm cảnh báo nhân viên y tế sớm.
- **Vấn đề hoặc sự kiện đáng chú ý:** Kiểm định độc lập cho thấy hiệu năng thấp hơn báo cáo trước đó của nhà phát triển. Ngưỡng cảnh báo tạo nhiều cảnh báo trong khi vẫn bỏ sót phần lớn ca sepsis trong tập nghiên cứu, đặt ra rủi ro bỏ sót và mệt mỏi cảnh báo.
- **Số liệu có nguồn:** Nghiên cứu gồm **27.697 bệnh nhân, 38.455 lượt nhập viện**; sepsis xảy ra trong **2.552 lượt (7%)**. AUC ở cấp lượt nhập viện là **0,63** (KTC 95%: 0,62–0,64). Với điểm ESM ≥6, mô hình tạo cảnh báo ở **6.971/38.455 lượt (18%)**, nhưng không phát hiện **1.709/2.552 lượt sepsis (67%)**. Nhóm nghiên cứu cũng nêu đây là đánh giá hồi cứu; các cảnh báo mô phỏng trong phân tích không đồng nghĩa mọi cảnh báo đã được gửi tới bác sĩ.
- **Nguồn:** [Wong et al., “External Validation of a Widely Implemented Proprietary Sepsis Prediction Model in Hospitalized Patients,” JAMA Internal Medicine 181(8), 1065–1070 (21/06/2021), doi:10.1001/jamainternmed.2021.2626](https://jamanetwork.com/journals/jamainternalmedicine/fullarticle/2781307).
- **Phân biệt bằng chứng và nhận định:** Bằng chứng là hiệu năng kiểm định và số lượt bị bỏ sót / được gắn cờ trong tập hồi cứu. Nhận định của tôi là nếu dùng điểm này như căn cứ chính để quyết định, cảnh báo giả có thể khiến nhân viên giảm chú ý và ca bị bỏ sót có thể không được đánh giá sớm. Nghiên cứu không kết luận ESM đã gây tổn hại cho các bệnh nhân trong mẫu; 60% trong số các trường hợp sepsis bị mô hình bỏ sót vẫn được dùng kháng sinh kịp thời theo tiêu chí của nghiên cứu.

### Harm Map Worksheet

| Trường | Phân tích của tôi |
| --- | --- |
| High-risk moment | Khi ESM ≥6 kích hoạt cảnh báo hoặc khi nhân viên dựa vào điểm thấp để giảm mức cảnh giác với bệnh nhân có dấu hiệu xấu đi. |
| Stakeholder bị ảnh hưởng | Bệnh nhân có nguy cơ sepsis; bác sĩ, điều dưỡng xử lý cảnh báo; bệnh viện triển khai mô hình. |
| Failure mode | **False negative / bỏ sót** (không gắn cờ người bệnh có sepsis) và **false positive / cảnh báo quá mức** (gắn cờ người không thuộc nhóm sepsis theo định nghĩa nghiên cứu). |
| Layer bắt đầu lỗi | **Model.** Kiểm định ngoài cho thấy khả năng phân biệt và hiệu chuẩn yếu. Lớp vận hành/cấu hình ngưỡng có thể làm tăng mệt mỏi cảnh báo, nhưng nghiên cứu không quy kết nguyên nhân nội bộ cụ thể của mô hình. |
| Harm xảy ra là gì? | **Đã được chứng minh:** trong tập dữ liệu hồi cứu, ESM không gắn cờ 67% lượt sepsis tại ngưỡng ≥6 và gắn cờ 18% tổng lượt nhập viện. **Nguy cơ:** bỏ sót có thể làm chậm đánh giá/điều trị; lượng cảnh báo có thể làm giảm chú ý. Bài báo không xác nhận đây là hậu quả lâm sàng đã xảy ra do mô hình. |
| Harm lens | An toàn bệnh nhân; sức khỏe; áp lực công việc và khả năng duy trì chú ý của nhân viên y tế. |
| Severity | **Critical** nếu cảnh báo sai hoặc thiếu làm chậm xử trí sepsis ở người bệnh nặng; mức độ thực tế của hậu quả không được nghiên cứu này đo. |
| Scale | **Medium trong nghiên cứu được kiểm định:** một hệ thống y tế, 38.455 lượt nhập viện; nhưng sản phẩm được bài báo mô tả là triển khai tại hàng trăm bệnh viện Hoa Kỳ. Không lấy mẫu nghiên cứu làm con số người bị hại. |
| Probability | **High đối với lỗi dự đoán tại ngưỡng đã khảo sát:** 67% lượt sepsis không bị gắn cờ trong mẫu. Xác suất biến lỗi đó thành tổn hại lâm sàng chưa được đo. |
| Frequency | Lỗi có thể lặp lại mỗi lần điểm được tính/cảnh báo được kích hoạt; mô hình được tính mỗi 15 phút theo bài báo. |
| Vì sao? | Nghiên cứu nêu AUC 0,63, tỷ lệ bỏ sót 67% và cảnh báo ở 18% lượt nhập viện tại ngưỡng ≥6. Đây là hiệu năng hồi cứu tại một trung tâm, có định nghĩa sepsis và hạn chế riêng; nên cần kiểm định tại bệnh viện dự định sử dụng, giám sát cảnh báo và giữ đánh giá lâm sàng độc lập. |

## 4. Case study 3 — AI phân loại tổn thương da hoạt động kém hơn trên da sẫm màu (Daneshjou et al., 2022)

### Brief Case

- **Tổ chức / sản phẩm AI:** Nghiên cứu của Daneshjou và cộng sự kiểm tra ba thuật toán phân loại tổn thương lành tính/ác tính: ModelDerm, DeepDerm và mô hình dựa trên HAM10000. Đây là đánh giá nghiên cứu các mô hình dermatology AI, không phải một sự cố triển khai của một bệnh viện cụ thể.
- **Thời gian, địa điểm / bối cảnh:** Nhóm nghiên cứu xây dựng bộ ảnh DDI từ tổn thương được sinh thiết tại Stanford Clinics giai đoạn 2010–2020; công bố năm 2022.
- **AI được dùng để làm gì:** Hỗ trợ phân loại tổn thương da, có tiềm năng giúp sàng lọc nguy cơ ung thư da hoặc hỗ trợ bác sĩ không chuyên khoa.
- **Vấn đề hoặc sự kiện đáng chú ý:** Các mô hình hoạt động kém hơn khi kiểm tra trên bộ ảnh DDI có xác nhận mô bệnh học, đặc biệt với ảnh da sẫm màu và bệnh ít gặp. Kết quả cho thấy hiệu năng tốt trên bộ dữ liệu huấn luyện/kiểm thử ban đầu không đảm bảo hiệu năng tương tự trên nhóm bệnh nhân và ảnh khác.
- **Số liệu có nguồn:** DDI có **656 ảnh**: 208 ảnh FST I–II, 241 ảnh FST III–IV, 207 ảnh FST V–VI. Trên nhóm FST I–II so với FST V–VI, độ nhạy phát hiện ác tính của ModelDerm là **0,41 so với 0,12**; của DeepDerm là **0,69 so với 0,23**. DDI là bộ dữ liệu nghiên cứu được chọn từ ca sinh thiết, không đại diện đầy đủ mọi bệnh nhân hoặc ảnh ngoài đời. Tinh chỉnh DeepDerm/HAM10000 trên dữ liệu đa dạng giúp thu hẹp khoảng cách trong các thí nghiệm của nhóm.
- **Nguồn:** [Daneshjou et al., “Disparities in dermatology AI performance on a diverse, curated clinical image set,” Science Advances 8(32), eabq6147 (12/08/2022), doi:10.1126/sciadv.abq6147](https://pmc.ncbi.nlm.nih.gov/articles/PMC9374341/).
- **Phân biệt bằng chứng và nhận định:** Bằng chứng là độ nhạy và AUC khác nhau trên ảnh thuộc các nhóm tông da trong bộ DDI. Nhận định của tôi là nếu một công cụ tương tự được triển khai mà không kiểm định trên ảnh và bệnh nhân đa dạng, tổn thương ác tính ở người da sẫm màu có thể bị ưu tiên thấp hoặc đánh giá sai. Nghiên cứu là kiểm thử mô hình trên bộ ảnh, không đo số bệnh nhân bị chẩn đoán muộn ngoài đời.

### Harm Map Worksheet

| Trường | Phân tích của tôi |
| --- | --- |
| High-risk moment | Khi công cụ gợi ý tổn thương lành tính hoặc mức ưu tiên thấp và kết quả đó ảnh hưởng quyết định khám, sinh thiết hoặc chuyển bác sĩ da liễu. |
| Stakeholder bị ảnh hưởng | Người bệnh có da sẫm màu, đặc biệt người có tổn thương ác tính; bác sĩ dùng công cụ sàng lọc; cơ sở y tế áp dụng mô hình trên ảnh chụp. |
| Failure mode | **Domain shift / underdiagnosis:** thiếu đại diện trong dữ liệu và khác biệt về tông da, loại ảnh hoặc bệnh làm giảm khả năng phát hiện; nguy cơ false negative với tổn thương ác tính. |
| Layer bắt đầu lỗi | **Grounding / dữ liệu và Model.** DDI cho thấy thiếu đa dạng trong dữ liệu đánh giá trước đây và giảm hiệu năng ngoài bộ dữ liệu gốc; nghiên cứu không xác định một lỗi UX triển khai cụ thể. |
| Harm xảy ra là gì? | **Đã được chứng minh:** độ nhạy phát hiện ác tính thấp hơn trên nhóm FST V–VI với ModelDerm và DeepDerm trong DDI. **Nguy cơ:** tổn thương có thể bị đánh giá thấp, làm chậm sinh thiết/chẩn đoán. Nghiên cứu không đo hậu quả điều trị thực tế. |
| Harm lens | Công bằng theo tông da; an toàn và sức khỏe; tiếp cận chẩn đoán kịp thời. |
| Severity | **High.** Bỏ sót tổn thương ác tính có thể trì hoãn chẩn đoán ung thư; đây là mức độ hậu quả tiềm tàng, không phải kết luận rằng các ca trong nghiên cứu đã bị trì hoãn. |
| Scale | **Medium trong bằng chứng trực tiếp:** 656 ảnh tại một bộ dữ liệu nghiên cứu. Quy mô thực tế có thể lớn hơn nếu mô hình được dùng rộng rãi, nhưng bài báo không đo số người đã chịu tác động triển khai. |
| Probability | **High đối với chênh lệch hiệu năng trong tập thử nghiệm:** độ nhạy chênh rõ giữa nhóm tông da. Xác suất lỗi cho từng người bệnh ngoài tập dữ liệu chưa được xác định. |
| Frequency | Có thể lặp lại với mỗi ảnh/ca thuộc nhóm mà mô hình nhận diện kém; tần suất triển khai lâm sàng không được nghiên cứu báo cáo. |
| Vì sao? | Kết quả định lượng từ 656 ảnh và kiểm định ba mô hình là bằng chứng về chênh lệch hiệu năng, không phải tỷ lệ chẩn đoán muộn ngoài thực tế. Nghiên cứu cho thấy tinh chỉnh bằng dữ liệu đa dạng có thể thu hẹp khoảng cách; trước khi dùng lâm sàng cần kiểm định theo tông da, loại ảnh và loại bệnh, đồng thời yêu cầu bác sĩ xác nhận. |

## Kết luận của tôi

Ba case cho thấy rủi ro trong y tế có thể bắt nguồn từ mục tiêu dự đoán không phù hợp, chất lượng/độ đại diện dữ liệu, khả năng tổng quát hóa và cách con người sử dụng cảnh báo. Các con số hiệu năng không tự chứng minh đã có người bệnh bị tổn hại; cần phân biệt rõ lỗi đo được với hậu quả lâm sàng. Tôi cho rằng hệ thống AI y tế nên được kiểm định độc lập trên nhóm bệnh nhân đa dạng, giám sát sau triển khai, giải thích giới hạn cho nhân viên y tế và luôn có người chịu trách nhiệm rà soát quyết định quan trọng.
