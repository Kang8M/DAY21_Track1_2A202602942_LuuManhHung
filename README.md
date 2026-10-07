# Lab 21 — Phân tích rủi ro AI qua case study thực tế

- Họ và tên: Lưu Mạnh Hưng
- MSSV / mã học viên: 2A202602942
- Lớp: H201
- Ngành đã chọn: HR / tuyển dụng (AI sàng lọc CV, đánh giá hoặc hỗ trợ tuyển ứng viên)

### 1. Industry Risk Snapshot


| Nội dung                                       | Đánh giá của tôi và lý do                                                                                                                                                                                                                                                                        |
| ------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Những tác hại chính có thể xảy ra        | Ứng viên bị loại không công bằng theo giới tính, tuổi, chủng tộc (mất cơ hội việc làm); nhà tuyển dụng đối mặt rủi ro pháp lý và uy tín; ứng viên khó biết/khiếu nại vì quyết định loại thường không được giải thích.                                    |
| Mức độ high-stakes                           | **Cao.** Quyết định tuyển dụng ảnh hưởng trực tiếp đến thu nhập và sự nghiệp; hệ thống sàng lọc xử lý hàng loạt hồ sơ nên một lỗi hệ thống lặp lại ở quy mô lớn.                                                                                                    |
| Dữ liệu nhạy cảm có thể được sử dụng | Họ tên, ngày sinh/tuổi, giới tính, ảnh, địa chỉ, lịch sử học vấn và việc làm, đôi khi video phỏng vấn. Các thuộc tính này có thể bị mô hình dùng trực tiếp hoặc gián tiếp (proxy). Bài này chỉ dùng nguồn công khai, không đưa dữ liệu ứng viên thật. |
| Nhu cầu human review                           | **Cao.** Nhà tuyển dụng nên kiểm tra mẫu hồ sơ bị loại, audit thiên lệch định kỳ trước và sau triển khai, và để con người quyết định cuối cùng với các hồ sơ bị AI loại hoặc xếp thấp.                                                                            |

*Thấp/Trung bình/Cao là đánh giá định tính của tôi cho bài tập, không phải phân loại pháp lý.*

### 2. Case study 1 — Amazon: công cụ AI chấm CV thiên lệch với phụ nữ

#### Brief Case

- Tổ chức / sản phẩm AI: Amazon, công cụ tuyển dụng thử nghiệm nội bộ dùng machine learning chấm hồ sơ.
- Thời gian, địa điểm / bối cảnh: Phát triển từ 2014; đến 2015 phát hiện không trung lập về giới; Reuters đưa tin tháng 10/2018. Nội bộ Amazon, các vị trí kỹ thuật (software developer).
- AI được dùng để làm gì: Chấm điểm ứng viên từ 1 đến 5 sao để tự động hóa việc tìm ứng viên giỏi.
- Vấn đề hoặc sự kiện đáng chú ý: Mô hình học từ hồ sơ nộp vào Amazon trong 10 năm (phần lớn của nam giới) nên phạt hồ sơ chứa từ "women's" (ví dụ "women's chess club captain") và hạ điểm sinh viên tốt nghiệp từ hai trường đại học nữ. Amazon chỉnh sửa để trung lập với các từ đó nhưng không đảm bảo các thiên lệch khác không xuất hiện; nhóm dự án sau đó bị giải tán.
- Số liệu có nguồn: Dữ liệu huấn luyện trải 10 năm hồ sơ; thang điểm 1–5 sao; 2 trường đại học nữ bị hạ điểm; thời gian phát triển từ 2014 đến khi phát hiện vấn đề năm 2015 (theo Reuters qua các bản tường thuật bên dưới).
- Nguồn: "Amazon scraps secret AI recruiting tool that showed bias against women" — Reuters — 10/10/2018 — bản đăng lại: https://www.cnbc.com/2018/10/10/amazon-scraps-a-secret-ai-recruiting-tool-that-showed-bias-against-women.html ; tổng hợp: https://www.recruiter.co.uk/news/2018/10/amazon-abandons-ai-recruiting-tool-it-discriminated-against-women ; hồ sơ sự cố: https://incidentdatabase.ai/reports/611
- Phân biệt bằng chứng và nhận định: Nguồn xác nhận cơ chế thiên lệch và việc dự án bị bỏ. Tôi nhận định là Amazon không cho biết công cụ có từng là yếu tố quyết định duy nhất trong tuyển dụng hay không; đây là điểm chưa rõ, tôi không suy diễn thêm. Tôi chưa mở được trang Reuters gốc (bị chặn), nên các chi tiết lấy từ các bản đăng lại/tổng hợp.

#### Harm Map Worksheet


| Trường                     | Phân tích của tôi                                                                                                                                                                                                                                                                                                                     |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| High-risk moment             | Bước sàng lọc CV tự động: điểm sao của AI quyết định hồ sơ nào được nhà tuyển dụng xem tiếp.                                                                                                                                                                                                                       |
| Stakeholder bị ảnh hưởng | Ứng viên nữ cho vị trí kỹ thuật; nhà tuyển dụng (Amazon); các ứng viên khác bị xếp hạng sai lệch.                                                                                                                                                                                                                       |
| Failure mode                 | Bias / fairness.                                                                                                                                                                                                                                                                                                                          |
| Layer bắt đầu lỗi        | Model (kèm dữ liệu huấn luyện): mô hình học mẫu thiên lệch từ dữ liệu lịch sử toàn nam. Chưa đủ bằng chứng về lỗi ở các layer UX/Safety.                                                                                                                                                                        |
| Harm xảy ra là gì?        | Ứng viên nữ có nguy cơ bị hạ điểm vô lý và mất cơ hội được xem xét. Nguồn xác nhận cơ chế hạ điểm; tôi không có nguồn cho việc cụ thể ứng viên nào bị từ chối thực sự, nên harm là**nguy cơ đã được chứng minh ở mức hành vi mô hình**, chưa xác nhận hậu quả cho cá nhân. |
| Harm lens                    | Opportunity loss (mất cơ hội).                                                                                                                                                                                                                                                                                                         |
| Severity                     | Medium–High (ảnh hưởng cơ hội nghề nghiệp, nhưng không rõ hậu quả thực tế do công cụ chưa dùng làm quyết định duy nhất). Chọn:**High**.                                                                                                                                                                          |
| Scale                        | Chưa đủ dữ liệu về số ứng viên bị ảnh hưởng; công cụ chỉ là thử nghiệm nội bộ nên ước tính của tôi: Medium.                                                                                                                                                                                                    |
| Probability                  | Cao với phụ nữ trong nhóm hồ sơ kỹ thuật (nhận định của tôi dựa trên việc mô hình phạt trực tiếp từ khóa liên quan giới).                                                                                                                                                                                        |
| Frequency                    | Chưa đủ dữ liệu để đánh giá; thiên lệch nằm trong mô hình nên sẽ lặp lại ở mọi lần chấm hồ sơ phù hợp.                                                                                                                                                                                                         |
| Vì sao?                     | Dữ liệu 10 năm lệch nam khiến mô hình coi đặc điểm của nam là tín hiệu tốt. Việc sửa từng từ khóa không loại bỏ được proxy khác. Giới hạn: tôi dựa trên tường thuật của Reuters, Amazon không công bố chi tiết kỹ thuật.                                                                       |

### 3. Case study 2 — iTutorGroup: phần mềm tuyển dụng tự động loại ứng viên lớn tuổi

#### Brief Case

- Tổ chức / sản phẩm AI: iTutorGroup và các công ty liên kết; phần mềm nhận hồ sơ ứng tuyển gia sư trực tuyến.
- Thời gian, địa điểm / bối cảnh: Cuối tháng 3 đến đầu tháng 4/2020, tuyển gia sư tại Mỹ; EEOC khởi kiện, tòa liên bang phê chuẩn consent decree ngày 8/9/2023.
- AI được dùng để làm gì: Sàng lọc hồ sơ ứng tuyển gia sư tự động.
- Vấn đề hoặc sự kiện đáng chú ý: EEOC cáo buộc phần mềm được lập trình để tự động từ chối ứng viên nữ trên 55 tuổi và nam trên 60 tuổi (vi phạm ADEA). Ứng viên khởi kiện bị từ chối ngay khi nhập ngày sinh thật, hôm sau nộp lại với ngày sinh trẻ hơn (hồ sơ còn lại giống hệt) thì được mời phỏng vấn.
- Số liệu có nguồn: Hơn 200 ứng viên đủ điều kiện từ 55 tuổi trở lên bị từ chối trong khoảng cuối tháng 3 đến đầu tháng 4/2020 (cáo buộc của EEOC); mức dàn xếp 365.000 USD.
- Nguồn: Tường thuật thông cáo EEOC "iTutorGroup to Pay $365,000 to Settle EEOC Discriminatory Hiring Suit" (8/2023) — Duane Morris: https://blogs.duanemorris.com/classactiondefense/2023/08/11/eeoc-settles-its-first-discrimination-lawsuit-involving-artificial-intelligence-hiring-software/ ; Akin Gump: https://www.akingump.com/en/insights/blogs/ag-data-dive/eeoc-settles-over-recruiting-software-in-possible-first-ever-ai-related-case ; Norton Rose Fulbright: https://nortonrosefulbright.com/en-nl/knowledge/publications/2ec12415/us-eeocs-first-settlement-in-ai-hiring-discrimination
- Phân biệt bằng chứng và nhận định: Các số liệu là **cáo buộc của EEOC** được giải quyết bằng dàn xếp, không phải phán quyết về lỗi. Đây là quy tắc lập trình cứng chứ không rõ có phải "học máy"; tôi xem là case về phần mềm tuyển dụng tự động. Tôi chưa mở được trang EEOC gốc (bị chặn), nên dựa vào các bài phân tích pháp lý.

#### Harm Map Worksheet


| Trường                     | Phân tích của tôi                                                                                                                                                                               |
| ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| High-risk moment             | Ngay khi nộp hồ sơ, phần mềm đọc ngày sinh và tự động từ chối trước khi con người xem.                                                                                            |
| Stakeholder bị ảnh hưởng | Ứng viên lớn tuổi (nữ >55, nam >60); iTutorGroup (rủi ro pháp lý); EEOC/cơ quan quản lý.                                                                                                 |
| Failure mode                 | Bias / fairness (phân biệt tuổi).                                                                                                                                                                |
| Layer bắt đầu lỗi        | Grounding / quy tắc hệ thống: tiêu chí loại được cài sẵn trong phần mềm; không phải lỗi do mô hình tự học. Chưa đủ bằng chứng về chi tiết kiến trúc.                   |
| Harm xảy ra là gì?        | Hơn 200 ứng viên đủ điều kiện bị từ chối vì tuổi (theo cáo buộc EEOC) và mất cơ hội việc làm; đây là hậu quả đã được cáo buộc/dàn xếp, chưa phải phán quyết. |
| Harm lens                    | Opportunity loss (mất cơ hội), kèm dignity loss (nhận định của tôi).                                                                                                                       |
| Severity                     | Medium–High; chọn:**High** (bị từ chối việc làm vì tuổi, trái luật).                                                                                                                     |
| Scale                        | Hơn 200 ứng viên trong khoảng 1 tháng (theo cáo buộc).                                                                                                                                       |
| Probability                  | Cao với ứng viên thuộc nhóm tuổi bị loại, vì luật loại là tự động.                                                                                                                   |
| Frequency                    | Lặp lại với mọi hồ sơ thuộc nhóm đó trong giai đoạn cáo buộc; chưa có dữ liệu ngoài khoảng cuối 3 đến đầu 4/2020.                                                          |
| Vì sao?                     | Quy tắc tự động không có human review nên lỗi lặp lại không bị phát hiện, cho tới khi một ứng viên thử đổi ngày sinh. Giới hạn: số liệu là cáo buộc đã dàn xếp.    |

### 4. Case study 3 — Nghiên cứu Đại học Washington: LLM xếp hạng CV thiên lệch chủng tộc và giới tính

#### Brief Case

- Tổ chức / sản phẩm AI: Nghiên cứu của University of Washington (Kyra Wilson, Aylin Caliskan) thử 3 mô hình ngôn ngữ lớn của Mistral AI, Salesforce và Contextual AI.
- Thời gian, địa điểm / bối cảnh: Công bố tại AAAI/ACM Conference on AI, Ethics, and Society (10/2024); tin của trường ngày 31/10/2024.
- AI được dùng để làm gì: Xếp hạng CV theo mức phù hợp với mô tả công việc (mô phỏng sàng lọc CV bằng LLM).
- Vấn đề hoặc sự kiện đáng chú ý: Khi đổi 120 tên gắn với nam/nữ da trắng và da đen trên cùng CV thật, các mô hình ưu tiên tên gắn với người da trắng và nam giới.
- Số liệu có nguồn: Hơn 550 CV thật, hơn 500 mô tả công việc thuộc 9 nghề, hơn 3 triệu lượt so sánh; tên gắn với người da trắng được ưu tiên 85% so với 9% cho tên gắn với người da đen; tên nam được ưu tiên 52% so với 11% cho tên nữ; tên nam da đen không bao giờ được ưu tiên hơn tên nam da trắng.
- Nguồn: "AI tools show biases in ranking job applicants' names according to perceived race and gender" — University of Washington News — 31/10/2024 — https://www.washington.edu/news/2024/10/31/ai-bias-resume-screening-race-gender/ (đã đọc trực tiếp).
- Phân biệt bằng chứng và nhận định: Nguồn xác nhận kết quả thí nghiệm trong điều kiện kiểm soát. Việc các nhà tuyển dụng thực tế dùng đúng những mô hình này và mức tác động thực tế lên ứng viên là điều **nguồn chưa chứng minh**; đây là nguy cơ tôi suy luận.

#### Harm Map Worksheet


| Trường                     | Phân tích của tôi                                                                                                                                                                                                                |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| High-risk moment             | Nhà tuyển dụng dùng LLM để xếp hạng/lọc hàng loạt CV trước khi người đọc.                                                                                                                                           |
| Stakeholder bị ảnh hưởng | Ứng viên da đen và ứng viên nữ (đặc biệt nam da đen); nhà tuyển dụng dùng công cụ.                                                                                                                                  |
| Failure mode                 | Bias / fairness.                                                                                                                                                                                                                     |
| Layer bắt đầu lỗi        | Model: thiên lệch xuất hiện ở mô hình nền khi đổi riêng tên. Chưa đủ bằng chứng về các layer khác.                                                                                                               |
| Harm xảy ra là gì?        | Trong thí nghiệm, CV mang tên gắn với nhóm bị thiệt thòi bị xếp thấp. Harm với ứng viên thật là**nguy cơ**, không phải hậu quả đã được xác nhận.                                                        |
| Harm lens                    | Opportunity loss (mất cơ hội).                                                                                                                                                                                                    |
| Severity                     | High (nếu bị dùng thật thì ảnh hưởng cơ hội việc làm); đánh giá của tôi.                                                                                                                                            |
| Scale                        | Thí nghiệm hơn 3 triệu so sánh; quy mô thực tế chưa đủ dữ liệu, nhận định: Medium–High do LLM được dùng rộng rãi trong tuyển dụng (tác giả nhận xét việc dùng AI trong tuyển dụng đã phổ biến). |
| Probability                  | Cao trong điều kiện thử nghiệm với các mô hình được thử; xác suất ngoài thực tế chưa đủ dữ liệu.                                                                                                              |
| Frequency                    | Chưa đủ dữ liệu ngoài thí nghiệm; trong thí nghiệm, xu hướng lặp lại trên số lượng lớn so sánh.                                                                                                                  |
| Vì sao?                     | Mô hình học thiên lệch từ dữ liệu văn bản; tên là tín hiệu duy nhất bị đổi nên chênh lệch có thể quy cho tên. Giới hạn: chỉ 3 mô hình, mô phỏng, chưa phải thực tế tuyển dụng.                  |
