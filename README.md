#Biên dịch từ lient-side và server-side : Tất cả ưu điểm và nhược điểm của chúng 
* Client-side vs. server-side rendering: why it’s not all black and white
* Viết bởi Juan Vega là kỹ sư phần mềm là người viết ra trang web  https://vuereactor.com và https://juanmvega.com
* Dịch bởi Nguyễn Xuân Quang
* Nguồn https://medium.freecodecamp.org/what-exactly-is-client-side-rendering-and-hows-it-different-from-server-side-rendering-bd5c786b340d

### Bài dịch
Kể từ lúc bắt đầu,phương pháp thông thường để nhận HTML của bạn lên màn hình bằng cách sử dụng rending từ phía sever-side. Đó là cách duy nhất. Bạn đã tải các trang .html trên máy chủ, sau đó máy chủ của bạn đã chuyển và chuyển chúng thành các tài liệu hữu ích trên trình duyệt của bạn.

Phản ánh phía máy chủ đã làm việc tuyệt vời vào thời điểm đó, vì hầu hết các trang web chủ yếu chỉ để hiển thị các hình ảnh và văn bản tĩnh, với một ít tương tác.

Kể từ hôm nay sẽ không còn trường hợp đó nữa. Bạn có thể lý giải rằng các trang web bây giờ giống như các ứng dụng giả mạo được chuyển hướng từ các trang web. Bạn có thể sử dụng chúng để gửi tin nhắn, cập nhật thông tin trực tuyến, cửa hàng, và nhiều hơn nữa. Các trang web ngày càng được nâng cấp hơn để sử dụng.

Vì vậy, nó làm cho ta cảm thấy rằng server-side rendering đang bắt đầu chậm chạp ở phía sau với phương pháp phát triển ngày càng tăng của các trang web hiển thị ở phía client.

Vậy phương pháp nào là lựa chọn tốt hơn? Như với hầu hết mọi thứ trong phát triển, nó thực sự phụ thuộc vào những gì bạn đang lập kế hoạch làm với trang web của bạn. Bạn cần phải hiểu những ưu và khuyết điểm, sau đó quyết định cho mình những tuyến đường nào là tốt nhất cho bạn.

####Cách làm việc của server-side rendering

Server-side rendering là phương pháp phổ biến nhất để hiển thị thông tin lên màn hình. Nó hoạt động bằng cách chuyển đổi các tệp HTML trong máy chủ thành thông tin có thể sử dụng được cho trình duyệt.

Bất cứ khi nào bạn truy cập trang web, trình duyệt của bạn sẽ yêu cầu máy chủ chứa nội dung của trang web. Yêu cầu thường chỉ mất vài mili giây, nhưng nó phụ thuộc vào nhiều các yếu tố như:

* Tốc độ internet của bạn
* Vị trí của sever
* Có bao nhiêu người dùng đang truy cập trang web cùng một thời điểm
* Và làm thế nào tối ưu hóa trang web, ít tên

Khi yêu cầu được xử lý hoàn tất, trình duyệt của bạn sẽ trả lại HTML hoàn chỉnh và hiển thị trên màn hình. Nếu bạn quyết định truy cập một trang khác trên trang web, trình duyệt của bạn sẽ lại yêu cầu một thông tin mới. Điều này sẽ xảy ra mỗi lần bạn truy cập trang mà trình duyệt của bạn không có phiên bản được lưu trong bộ nhớ cache.

Không có vấn đề gì nếu trang mới chỉ có một vài mục khác với trang hiện tại, trình duyệt sẽ trả về toàn bộ trang mới và sẽ tải lại trang từ đầu.

Lấy ví dụ như tài liệu HTML này đã được đặt trong một máy chủ tưởng tượng với một địa chỉ HTTP của example.testsite.com .

```javascript
    <!DOCTYPE html>
    <html>
      <head>
        <meta charset="utf-8">
        <title>Example Website</title>
      </head>
      <body>
        <h1>My Website</h1>
        <p>This is an example of my new website</p>
        <a href="http://example.testsite.com/other.html.">Link</a>
      </body>
    </html> 
```  

Nếu bạn gõ địa chỉ của trang web ví dụ vào URL của trình duyệt ảo, trình duyệt tưởng tượng của bạn sẽ yêu cầu máy chủ đang được sử dụng bởi URL đó và mong đợi phản hồi của một số văn bản để hiển thị trên trình duyệt. Trong trường hợp này, những gì bạn nhìn thấy trực quan sẽ là tiêu đề, nội dung đoạn và liên kết.

Bây giờ, giả sử rằng bạn muốn nhấp vào liên kết từ trang được hiển thị có chứa mã sau đây.

```javascript

    <!DOCTYPE html>
    <html>
      <head>
        <meta charset="utf-8">
        <title>Example Website</title>
      </head>
      <body>
        <h1>My Website</h1>
        <p>This is an example of my new website</p>
        <p>This is some more content from the other.html</p>
      </body>
    </html> 
    
```

Sự khác biệt duy nhất giữa trang trước và trang này là trang này không có liên kết và thay vào đó có một đoạn văn khác. Logic sẽ chỉ ra rằng chỉ có nội dung mới nên được trả lại và phần còn lại nên được để lại một mình. Ôi không, đó không phải là cách mà server-side rendering làm việc. Điều thực sự sẽ xảy ra là toàn bộ trang mới sẽ được hiển thị, chứ không chỉ nội dung mới.

Mặc dù nó có thể không phải là một vấn đề lớn đối với hai ví dụ này, hầu hết các trang web không đơn giản như vậy. Các trang web hiện đại có hàng trăm dòng mã và phức tạp hơn nhiều. Bây giờ hãy tưởng tượng duyệt một trang web và phải chờ cho mỗi trang và mỗi trang để render khi điều hướng trang web. Nếu bạn đã từng truy cập vào một trang web WordPress, bạn đã thấy họ chậm chạp đến mức nào. Đây là một trong những lý do tại sao.

Về lợi ích, sever-side rending rất tốt cho SEO. Nội dung của bạn có mặt trước khi bạn lấy nó, vì vậy các công cụ tìm kiếm có thể chỉ mục nó và thu thập thông tin nó tốt. Điều đó không phù hợp với client-side rendering. Không đơn giản tý nào. 

#### Cách mà client-side rendering làm việc

Khi các nhà lập trình viên nói về client-side rendering, họ đang nói về việc hiển thị nội dung trong trình duyệt bằng cách sử dụng JavaScript. Vì vậy, thay vì nhận được tất cả nội dung từ chính tài liệu HTML, bạn sẽ nhận được một tài liệu khung HTML với file JavaScript sẽ hiển thị phần còn lại của trang bằng trình duyệt.

Đây là một cách tiếp cận tương đối mới đối với việc hiển thị các trang web, và nó không thực sự trở nên phổ biến cho đến khi các thư viện JavaScript bắt đầu kết hợp nó vào phong cách phát triển của họ. Một số ví dụ nổi bật là Vue.js và React.js,<a href='https://medium.freecodecamp.org/reacts-jsx-vs-vue-s-templates-a-showdown-on-the-front-end-b00a70470409'> mà tác giả đã viết về nó .</a>

Quay lại trang trước, example.testsite.com , giả sử rằng bây giờ bạn có tệp index.html với các dòng mã sau.

```javascript

    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8">
      <title>Example Website</title>
    </head>
    <body>
      <div id="root">
        <app></app>
      </div>
      <script src="https://vuejs.org"type="text/javascript"></script>
      <script src="location/of/app.js"type="text/javascript"></script>
    </body>
    </html>
     
```

Bạn có thể thấy ngay rằng có một số thay đổi lớn trong cách index.hmtl hoạt động khi render bằng cách sử dụng client.

Đối với người mới bắt đầu, thay vì có nội dung bên trong file HTML, bạn có một vùng chứa div với id của gốc. Bạn cũng có hai phần tử kịch bản ngay phía trên thẻ đóng phần. Một tệp sẽ tải thư viện JavaScript Vue.js và tệp sẽ tải tệp tin có tên app.js.

Điều này hoàn toàn khác so với sử dụng server-side rendering bởi vì máy chủ bây giờ chỉ chịu trách nhiệm tải dữ liệu trừ đi của trang web. Cái chính trong các bản mẩu. Mọi thứ khác được xử lý bởi thư viện client-side JavaScript, trong trường hợp này là Vue.js và tùy chỉnh code Javascript.

Nếu bạn muốn yêu cầu URL chỉ với mã ở trên, bạn sẽ nhận được một màn hình trống. Không có gì để tải vì nội dung thực sự cần được hiển thị bằng cách sử dụng JavaScript.


Để khắc phục điều đó, bạn sẽ đặt các dòng mã sau vào tệp app.js.

```javascript

    var data = {
            title:"My Website",
            message:"This is an example of my new website"
          }
      Vue.component('app', {
        template:
        `
        <div>
        <h1>{{title}}</h1>
        <p id="moreContent">{{message}}</p>
        <a v-on:click='newContent'>Link</a>
        </div>
        `,
        data: function() {
          return data;
        },
        methods:{
          newContent: function(){
            var node = document.createElement('p');
            var textNode = document.createTextNode('This is some more content from the other.html');
            node.appendChild(textNode);
            document.getElementById('moreContent').appendChild(node);
          }
        }
      })
      new Vue({
        el: '#root',
      });

```

Bây giờ nếu bạn truy cập URL, bạn sẽ thấy cùng một nội dung như bạn đã làm ví dụ phía sever. Sự khác biệt chính là nếu bạn nhấp vào liên kết trang để tải nhiều nội dung hơn, trình duyệt sẽ không thực hiện một yêu cầu khác cho sever. Bạn đang hiển thị các mục bằng trình duyệt, do đó thay vào đó sẽ sử dụng JavaScript để tải nội dung mới và Vue.js sẽ đảm bảo chỉ có nội dung mới được hiển thị. Khung HTML sẽ được giữ lại.

Điều này nhanh hơn nhiều vì bạn chỉ tải một phần rất nhỏ của trang để tìm tải nội dung mới thay vì tải toàn bộ trang.

Tuy nhiên vẫn có một số trường hợp ngoại lệ khi sử dụng client-side rendering. Vì nội dung không được hiển thị cho đến khi trang được tải trên trình duyệt, SEO cho trang web sẽ có kết quả tốt hơn. Có nhiều cách để giải quyết vấn đề này, nhưng nó không dễ dàng như với server-side rendering.

Một điều nữa cần lưu ý là trang web / ứng dụng của bạn sẽ không thể tải cho đến khi tất cả JavaScript được tải xuống trình duyệt. Điều đó có ý nghĩa, vì nó chứa tất cả nội dung cần thiết. Nếu người dùng của bạn đang sử dụng kết nối internet chậm, có thể làm cho thời gian tải ban đầu của họ chậm một chút.


### Những ưu và nhược điểm của mỗi cách tiếp cận 

Nên đó là điều bạn có.  Đó là những khác biệt chính giữa server-side và client-side rendering. Chỉ có bạn là lập trình viên có thể quyết định lựa chọn nào là tốt nhất cho trang web hoặc ứng dụng của mình.

Dưới đây là những ưu và khuyết điểm của từng phương pháp:

####Ưu diểm của phía sever :

 #####1. Các công cụ tìm kiếm có thể thu thập dữ liệu trang web để SEO tốt hơn.
 
 #####2. Tải trang ban đầu nhanh hơn.
 
 #####3. Tuyệt vời cho các trang web tĩnh.

#### Nhược điểm của sever :

 #####1. Truy vấn sever thường xuyên.
 
 #####2. Hiển thị trang chậm.
 
 #####3. Tải lại toàn bộ trang.
 
 #####4. Tương tác web không được tốt.
 
#### Ưu điểm của client :

 ##### 1. Tương tác nhiều trang web
 
 ##### 2. Hiển thị trang web nhanh sau khi tải lần đầu.
 
 ##### 3. Tuyệt vời cho các ứng dụng web.
 
 ##### 4. Chọn thư viện JavaScript mạnh mẽ.
 
####   Nhược điểm của client :

 #####  1. SEO thấp nếu không thực hiện chính xác.
 
 #####  2. Tải lần đầu có thể cần nhiều thời gian hơn.
 
 #####  3. Trong hầu hết các trường hợp, yêu cầu một thư viện bên ngoài.
 
 
 Nếu bạn muốn tìm hiểu thêm về Vue.js, hãy kiểm tra VueReactor.com. Bạn cũng có thể truy cập juanmvega.com để cập nhật với những câu chuyện mới nhất của tác giả.
 
THE END




