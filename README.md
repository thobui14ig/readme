## @Before()
- Mục đích: Sử dụng cho các trường hợp cần thực hiện trước function chạy chính(ví dụ transform, CRUD, modified các biến đầu vào) mục đích sử dụng tương tự như các interceptor thực thi trước khi vào controller.
- Đối số của @Before() là các function 
- Cách sử dụng:
  - Ví dụ:
  ```typescript
    import { BeforeType } from "@libs/decorators/before";
    import getService from "@libs/hellpers/get-service"
    import { KhamService } from "modules/kham/kham.service"

    async function BeforeDecoratorName ({ args }: BeforeType){
      const [param, query] = args;
      const khamService = await getService(KhamService);
      const connection = await getService(DataSource);
    };

    export default BeforeDecoratorName
  ```
    - BeforeDecoratorName: Tên của decorator.
    - args: là các tham số được truyền vào function chính được thực thi. Là 1 mảng.
    - BeforeType: kiểu dữ liệu bắt buộc cho các tham số của BeforeDecoratorName.
    - khamService: Là class khamService, là cách gọi 1 service trong decorator.
    - connection: Gọi connection DataSource để chạy các câu truy vấn bằng raw query.
  - Cách gọi trong @Before():
  ```typescript
    @Before(BeforeDecoratorName)
    async handle(@Param() param: any, @Query() query, @Body() body) {
      return true
    }
  ```
    - BeforeDecoratorName: Là tên của decorator được khai báo ở trên.
    - handle: Là function chạy chính, các hành động trong BeforeDecoratorName sẽ được gọi trước khi vào handle.
- Nhiều function trong @Before():
  ```typescript
    @Before(BeforeDecoratorNameV1, BeforeDecoratorNameV2)
    async handle(@Param() param: any, @Query() query, @Body() body) {
      return true
    }
  ```