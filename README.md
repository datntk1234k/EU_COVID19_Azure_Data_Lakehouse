# ETL Azure Data Factory Project on Covid-19

Dự án này tập trung vào việc thu thập và chuyển đổi dữ liệu Covid-19 từ nhiều nguồn khác nhau để cung cấp cái nhìn toàn diện về tác động của COVID-19 đối với khu vực Châu Âu trong suốt năm 2020. Dữ liệu được thu thập từ trang web ECDC và các nguồn khác, sau đó chuyển đổi qua các thành phần của Azure Data Factory (ADF), HDInsight và Databricks, cuối cùng được lưu trữ trong SQL Data Warehouse. Nhóm Analytics sẽ sử dụng dữ liệu này để rút ra các phân tích và dự đoán hữu ích.

## Nhiệm vụ
1. Thu thập dữ liệu từ nhiều nguồn khác nhau.
2. Dọn dẹp và chuyển đổi dữ liệu để đảm bảo chất lượng và phù hợp với các mục tiêu phân tích.
3. Tải dữ liệu đã xử lý vào kho lưu trữ trung tâm như Azure Data Lake hoặc SQL Data Warehouse.
4. Sử dụng dữ liệu này trong các công cụ BI như Power BI để phân tích chi tiết các số liệu Covid-19 (số ca nhiễm, tỷ lệ tử vong, ca nhập viện, ICU, số lượng xét nghiệm).
5. Tạo mô hình ML để dự đoán sự lây lan của COVID-19 trong khu vực Châu Âu.

## Nguồn Dữ Liệu
- **ECDC**: [Trang web ECDC](https://www.ecdc.europa.eu/en/covid-19)
- **Dữ liệu dân số**: Từ Azure Blob Storage

## Điểm Đích
- **Azure Data Lake Gen2 Storage**: Lưu trữ trung tâm cho dữ liệu đã xử lý.

## Công Cụ Sử Dụng
- **Tích hợp/Dung nạp dữ liệu**: ADF Data Flows trong Azure Data Factory
- **Chuyển đổi dữ liệu**:
  - ADF Data Flows
  - Databricks
- **Giải pháp kho dữ liệu**: Azure SQL Database
- **Trực quan hóa**: Power BI Desktop

## Môi Trường
- **Azure Subscription**
- **Azure Data Factory**
- **Azure Blob Storage Account**
- **Data Lake Storage Gen2**
- **Azure SQL Database**
- **Azure Databricks Cluster**
- **HDInsight Cluster**
