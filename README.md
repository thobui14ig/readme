## @Before()
- Mục đích: Sử dụng cho các trường hợp cần thực hiện trước function chạy chính(ví dụ transform, CRUD, modified các biến đầu vào) mục đích sử dụng tương tự như các interceptor thực thi trước khi vào controller.
- Đối số của @Before() là các function.
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
    1. BeforeDecoratorName: Là tên của decorator được khai báo ở trên.
    2. handle: Là function chạy chính, các hành động trong BeforeDecoratorName sẽ được gọi trước khi vào handle.
- Nhiều function trong @Before():
  ```typescript
    @Before(BeforeDecoratorNameV1, BeforeDecoratorNameV2)
    async handle(@Param() param: any, @Query() query, @Body() body) {
      return true
    }
  ```

## @Around()
- Mục đích: Sử dụng cho các trường hợp cần thực hiện trước và sau function chạy chính(ví dụ transform, CRUD, modified các biến đầu vào) mục đích sử dụng tương tự như các interceptor thực thi trước và sau khi vào controller. Các hành động thực hiện sau muốn lấy lại giá trị của hành động trước đó trong 1 function.
- Đối số của @Around() là 1 function duy nhất.
- Cách sử dụng:
  - Ví dụ:
  ```typescript
    import { AroundType } from "@libs/decorators/around";

    async function AroundDecoratorName ({ args, proceed  }: AroundType){
      const khamService = await getService(KhamService);
      const connection = await getService(DataSource);
      //Các hành động bạn muốn thực trước khi function chính được thực thi.
      const response = await proceed(...args); 
      //Các hành động bạn muốn thực hiện sau khi function chính được thực thi.
      return response
    };

    export default AroundDecoratorName
  ```
    - AroundDecoratorName: Tên của decorator.
    - args: là các tham số được truyền vào function chính được thực thi. Là 1 mảng.
    - AroundType: kiểu dữ liệu bắt buộc cho các tham số của AroundDecoratorName.
    - khamService: Là class khamService, là cách gọi 1 service trong decorator.
    - connection: Gọi connection DataSource để chạy các câu truy vấn bằng raw query.
    - const response = await proceed(...args): Function chính được gọi và trả kết quả về cho response.
  - Cách gọi trong @Around():
  ```typescript
    @Around(AroundDecoratorName)
    async handle(@Param() param: any, @Query() query, @Body() body) {
      return true
    }
  ```
    1. AroundDecoratorName: Là tên của decorator được khai báo ở trên.
    2. handle: Là function chạy chính, các hành động trong BeforeDecoratorName sẽ được gọi trước và sau khi vào handle.
- Nhiều function trong @Around():
  ```typescript
    @Around(AroundDecoratorNameV1)
    @Around(AroundDecoratorNameV2)
    @Around(AroundDecoratorNameV3)
    async handle(@Param() param: any, @Query() query, @Body() body) {
      return true
    }
  ```