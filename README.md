# learn-svelte

Tôi sử dụng repo này để theo dõi tất cả các bài học tôi đã học về `svelte`

## Tài liệu tham khảo

- https://svelte.dev/docs/introduction

- https://www.youtube.com/@naive-coder

## Công cụ playground trực tuyến

- https://svelte.dev/repl/hello-world?version=4.2.19

## Introduction

### Start a new project?

```svelte
npm create svelte@latest myapp cd myapp npm install npm run dev
```

`SvelteKit` sẽ xử lý việc gọi trình biên dịch `Svelte` để chuyển đổi các tệp `.svelte` của bạn thành các tệp `.js` tạo `DOM` và các tệp `.css` để định kiểu cho nó. Nó cũng cung cấp tất cả các thành phần khác mà bạn cần để xây dựng một ứng dụng web như máy chủ phát triển, định tuyến, triển khai, và hỗ trợ `SSR` (kết xuất phía máy chủ). `SvelteKit` sử dụng `Vite` để xây dựng mã của bạn.

### Alternatives to SvelteKit

Nếu bạn không muốn sử dụng `SvelteKit` vì lý do nào đó, bạn cũng có thể sử dụng `Svelte` cùng với `Vite` (nhưng không có `SvelteKit`) bằng cách chạy lệnh

```svelte
npm create vite@latest
```

và chọn tùy chọn `svelte`. Với cách này, lệnh `npm run build` sẽ tạo ra các tệp `HTML`, `JS` và `CSS` trong thư mục `dist`. Tuy nhiên, trong hầu hết các trường hợp, bạn có thể cần chọn một thư viện định tuyến.

Ngoài ra, có các plugin cho tất cả các công cụ đóng gói web lớn để xử lý việc biên dịch `Svelte` — những `plugin` này sẽ tạo ra các tệp `.js` và `.css` mà bạn có thể chèn vào `HTML` của mình — nhưng hầu hết các `plugin` khác sẽ không hỗ trợ `SSR` (kết xuất phía máy chủ).

### Editor tooling

Nhóm phát triển `Svelte` duy trì một tiện ích mở rộng cho `Visual Studio Code` và cũng có các tích hợp với nhiều trình chỉnh sửa và công cụ khác.

### Getting help

- https://discord.com/channels/457912077277855764/onboarding
- https://stackoverflow.com/questions/tagged/svelte

## Svelte components

Các thành phần `components` là những khối xây dựng của các ứng dụng `Svelte`. Chúng được viết trong các tệp `.svelte`, sử dụng một siêu tập hợp của `HTML`.

Cả ba phần — `script`, `styles` và `markup` — đều là tùy chọn.

```svelte
<script>
	// Chứa logic JavaScript sẽ chạy khi một instance của component được tạo ra.
</script>

<!-- Phần nội dung HTML (có thể có hoặc không có các phần tử). -->

<style>
	/* Chứa các kiểu dáng CSS dành riêng cho component này. */
</style>
```

### <script>

Khối `<script>` chứa `JavaScript` sẽ chạy khi một `instance` của `component` được tạo ra. Các biến được khai báo (hoặc nhập khẩu) ở cấp cao nhất có thể được 'nhìn thấy' từ phần `markup` của `component`. Có bốn quy tắc bổ sung:

### export tạo ra một prop của component.

`Svelte` sử dụng từ khóa `export` để đánh dấu khai báo biến như là một thuộc tính `property` hoặc `prop`, điều này có nghĩa là nó trở nên có thể truy cập được cho những người sử dụng `component` đó (xem phần về `property` và `props` để biết thêm thông tin).

```svelte
<script>
	export let foo;

	// Các giá trị được truyền vào dưới dạng props
	// sẽ ngay lập tức có sẵn
	console.log({ foo });
</script>
```

Bạn có thể chỉ định một giá trị mặc định ban đầu cho một `prop`. Giá trị này sẽ được sử dụng nếu người dùng `component` không chỉ định `prop` đó (hoặc nếu giá trị ban đầu của nó là `undefined`) khi khởi tạo `component`. Lưu ý rằng nếu giá trị của `props` sau đó được cập nhật, bất kỳ `prop` nào không có giá trị được chỉ định sẽ được đặt thành `undefined` (thay vì giá trị ban đầu của nó).

Trong chế độ phát triển (xem các tùy chọn biên dịch), một cảnh báo sẽ được in ra nếu không cung cấp giá trị mặc định ban đầu và người dùng không chỉ định giá trị. Để tắt cảnh báo này, hãy đảm bảo rằng một giá trị mặc định ban đầu được chỉ định, ngay cả khi nó là `undefined`.

```svelte
<script>
	export let bar = 'optional default initial value';
	export let baz = undefined;
</script>
```

Nếu bạn xuất một `const`, `class` , hoặc `function`, chúng sẽ là chỉ đọc (readonly) từ bên ngoài component. Tuy nhiên, các hàm vẫn có thể là giá trị của `prop`, như trong ví dụ dưới đây:

```svelte
<script>
	// đây là chỉ đọc (readonly)
	export const thisIs = 'readonly';

	/** @param {string} name */
	export function greet(name) {
		alert(`hello ${name}!`);
	}

	// đây là một prop
	export let format = (n) => n.toFixed(2);
</script>
```

Các `props` chỉ đọc (readonly) có thể được truy cập như các thuộc tính của phần tử, liên kết với `component` bằng cú pháp `bind:this syntax` (xem thêm phần https://svelte.dev/docs/component-directives#bind-this).

Bạn có thể sử dụng các từ khóa đã được đặt trước (reserved words) làm tên `props`.

```svelte
<script>
	/** @type {string} */
	let className;

	// tạo một thuộc tính `class`, mặc dù
	// nó là một từ khóa đã được đặt trước
	export { className as class };
</script>
```

### Khái niệm reactive

Trong Svelte, "reactive" (hoặc "reactivity") đề cập đến khả năng của hệ thống để tự động theo dõi và cập nhật giao diện người dùng (UI) khi trạng thái của `component` thay đổi. Điều này giúp bạn tránh phải cập nhật `UI` một cách thủ công, vì `Svelte` xử lý điều đó cho bạn.

### Assignments are 'reactive'

Để thay đổi trạng thái của `component` và kích hoạt việc tái kết xuất (re-render), bạn chỉ cần gán giá trị cho một biến được khai báo cục bộ.

Trong ví dụ dưới đây, khi bạn gọi hàm `handleClick`, giá trị của biến `count` sẽ được tăng lên 1. `Svelte` sẽ tự động nhận diện sự thay đổi và cập nhật giao diện người dùng (UI) nếu phần `markup` của `component` có tham chiếu đến `count`. Việc sử dụng biểu thức cập nhật (count += 1) hoặc phép gán giá trị (count = count + 1) đều có cùng hiệu ứng trong việc kích hoạt cập nhật `UI`.

```svelte
<script>
	let count = 0;

	function handleClick() {
		// gọi hàm này sẽ kích hoạt việc cập nhật
		// nếu phần markup tham chiếu đến `count`
		count = count + 1;
	}
</script>
```

Trong `Svelte`, vì cơ chế `reactivity` dựa trên các phép gán giá trị, các phương thức của mảng như `.push()` và `.splice()` sẽ không tự động kích hoạt việc cập nhật giao diện người dùng (UI). Để kích hoạt cập nhật, bạn cần thực hiện một phép gán bổ sung như arr = arr. Phép gán này báo cho `Svelte` rằng giá trị của `arr` đã thay đổi, và do đó `Svelte` sẽ cập nhật `UI` nếu `arr` được tham chiếu trong phần `markup`.

```svelte
<script>
	let arr = [0, 1];

	function handleClick() {
		// gọi phương thức này không kích hoạt cập nhật
		arr.push(2);
		// phép gán này sẽ kích hoạt cập nhật
		// nếu phần markup tham chiếu đến `arr`
		arr = arr;
	}
</script>
```

Trong `Svelte`, các khối <script> chỉ được thực thi khi `component` được tạo ra, vì vậy các phép gán trong khối `<script>` sẽ không tự động được thực hiện lại khi một prop được cập nhật. Trong ví dụ trên, biến name chỉ được thiết lập khi `component` được tạo ra, và nó sẽ không tự động cập nhật khi giá trị của `person` thay đổi.

Để theo dõi các thay đổi của một `prop`, bạn sẽ cần sử dụng một phương pháp khác, chẳng hạn như phản ứng với sự thay đổi của `prop` trong một khối `<script>`, như được minh họa trong phần ví dụ sau.

```svelte
<script>
	export let person;
	// điều này chỉ thiết lập `name` khi component được tạo ra
	// nó sẽ không cập nhật khi `person` thay đổi
	let { name } = person;
</script>
```

### $: marks a statement as reactive

- Câu lệnh `top-level`: Bất kỳ câu lệnh nào ở cấp cao nhất (không nằm trong một khối hoặc hàm) đều có thể được đánh dấu là `reactive` bằng cách thêm tiền tố `$:`.

- Thực thi câu lệnh `reactive`: Các câu lệnh `reactive` chạy sau khi các mã `script` khác được thực thi và trước khi giao diện `component` được `render`, bất cứ khi nào các giá trị mà chúng phụ thuộc thay đổi.

```svelte
<script>
	export let title;
	export let person;

	// điều này sẽ cập nhật document.title mỗi khi
	// prop title thay đổi
	$: document.title = title;

	$: {
		console.log(`nhiều câu lệnh có thể được kết hợp`);
		console.log(`tiêu đề hiện tại là ${title}`);
	}

	// điều này sẽ cập nhật name khi `person` thay đổi
	$: ({ name } = person);

	// không nên làm điều này. nó sẽ chạy trước dòng trên
	let name2 = name;
</script>
```

- Chỉ những giá trị trực tiếp xuất hiện trong khối `$:` mới trở thành các phụ thuộc của câu lệnh `reactive`.

```svelte
<script>
	let x = 0;
	let y = 0;

	/** @param {number} value */
	function yPlusAValue(value) {
		return value + y;
	}

	$: total = yPlusAValue(x);
</script>

Total: {total}
<button on:click={() => x++}> Increment X </button>

<button on:click={() => y++}> Increment Y </button>
```

Trong ví dụ trên, `total` chỉ được cập nhật khi giá trị của `x` thay đổi, vì `x` là giá trị trực tiếp trong biểu thức `yPlusAValue(x)`.
`y` không ảnh hưởng đến việc cập nhật `total` vì `y` không phải là một phần của khối `$:`.
Khi bạn nhấn nút để tăng giá trị của `x`, `total` sẽ được cập nhật vì `x` là một phần của biểu thức trong khối `$:`.

Khi bạn nhấn nút để tăng giá trị của `y`, `total` sẽ không được cập nhật ngay lập tức, vì `y` không phải là một phần của khối `$:` mà chỉ là một phần của hàm `yPlusAValue(x)` được gọi trong khối `$:`.

- Các khối reactive `$:` được sắp xếp và phân tích tĩnh tại thời điểm biên dịch. `Svelte` chỉ xem xét các biến được gán và sử dụng trực tiếp trong khối `$:` đó, không phải các biến trong các hàm được gọi từ khối đó.

```svelte
<script>
	let x = 0;
	let y = 0;

	/** @param {number} value */
	function setY(value) {
		y = value;
	}

	$: yDependent = y;
	$: setY(x);
</script>
```

Trong ví dụ này:

`yDependent = y;` là một khối `reactive` sẽ cập nhật giá trị của `yDependent` khi `y` thay đổi.

`setY(x);` là một khối `reactive` sẽ gọi hàm `setY` với giá trị của `x` và cập nhật `y` mỗi khi `x` thay đổi.

Do các khối `reactive` được phân tích tĩnh và sắp xếp theo thứ tự chúng xuất hiện trong mã nguồn, `setY(x);` sẽ được thực thi trước khi `yDependent = y;` được cập nhật.

Điều này có nghĩa là khi `x` thay đổi, `setY(x)` sẽ cập nhật `y`, nhưng `yDependent` sẽ không ngay lập tức phản ánh giá trị mới của `y` trong lần cập nhật kế tiếp.

`yDependent` sẽ chỉ cập nhật khi `y` thay đổi trực tiếp. Việc thay đổi `x` sẽ không trực tiếp làm `yDependent` cập nhật ngay lập tức vì sự cập nhật của `y` thông qua `setY(x)` sẽ không làm `yDependent` được cập nhật lại.

- Sắp xếp Các Khối `Reactive`:

Di chuyển câu lệnh `$: yDependent = y;` xuống dưới `$: setY(x);` sẽ làm cho `yDependent` được cập nhật khi `x` thay đổi, vì các khối `reactive` sẽ được thực thi theo thứ tự chúng xuất hiện. Khi `x` thay đổi, `setY(x)` sẽ cập nhật `y`, và sau đó `yDependent` sẽ được cập nhật dựa trên giá trị mới của `y`.
Khai Báo Biến Tự Động:

Nếu một câu lệnh chỉ bao gồm việc gán giá trị cho một biến chưa được khai báo, `Svelte` sẽ tự động chèn một câu lệnh khai báo `let` cho biến đó.
Trong ví dụ trên, biến `squared` và `cubed` không cần được khai báo trước vì `Svelte` tự động khai báo chúng.
Câu lệnh `$: squared = num _ num;` và `$: cubed = squared _ num;` sẽ tự động tạo ra các biến `squared` và `cubed` và theo dõi sự thay đổi của chúng, cập nhật giá trị khi `num` thay đổi.

```svelte
<script>
	/** @type {number} */
	export let num;

	// Chúng ta không cần khai báo `squared` và `cubed`
	// — Svelte sẽ tự động làm điều đó cho chúng ta
	$: squared = num * num;
	$: cubed = squared * num;
</script>
```

### Prefix stores with $ to access their values

- `Store` là một đối tượng cho phép truy cập giá trị một cách `reactive` qua một hợp đồng `store` đơn giản. Mô-đun `svelte/store` cung cấp các triển khai `store` cơ bản để thực hiện hợp đồng này.

- Truy Cập Giá Trị `Store`:

Bất kỳ khi nào bạn có một tham chiếu đến `store`, bạn có thể truy cập giá trị của nó trong `component` bằng cách thêm ký hiệu `$` trước tên `store`. Điều này khiến `Svelte` khai báo biến với tiền tố `$`, đăng ký vào `store` khi `component` được khởi tạo và hủy đăng ký khi cần thiết.

- Gán Giá Trị:

Việc gán giá trị cho các biến có tiền tố $ yêu cầu biến đó phải là một `writable store` và sẽ dẫn đến việc gọi phương thức `.set` của `store`.
Địa Điểm Khai Báo `Store`:

- `Store` phải được khai báo ở cấp cao nhất của `component` — không nằm trong một khối `if` hay `hàm`, chẳng hạn.
- Biến Cục Bộ:

Các biến cục bộ (không phải giá trị store) không nên có tiền tố `$`.

```svelte
<script>
	import { writable } from 'svelte/store';

	const count = writable(0);
	console.log($count); // in ra 0

	count.set(1);
	console.log($count); // in ra 1

	$count = 2;
	console.log($count); // in ra 2
</script>
```

### Tạo Stores Tùy Chỉnh Trong Svelte

- Bạn có thể tạo các store của riêng mình mà không cần dựa vào `svelte/store`, bằng cách triển khai hợp đồng `store`:

- `.subscribe Method:`

Một `store` phải chứa phương thức `.subscribe`, phương thức này phải chấp nhận một hàm đăng ký (subscription function) như đối số.
Hàm đăng ký này phải được gọi ngay lập tức và đồng bộ với giá trị hiện tại của store khi gọi `.subscribe`.
Tất cả các hàm đăng ký hiện tại của `store` phải được gọi đồng bộ mỗi khi giá trị của `store` thay đổi.

`Unsubscribe Function:`

Phương thức `.subscribe` phải trả về một hàm hủy đăng ký (unsubscribe function).
Việc gọi hàm hủy đăng ký phải dừng việc đăng ký, và hàm đăng ký tương ứng không được gọi lại bởi `store`.

- `.set Method (Tùy Chọn):`

Một `store` có thể tùy chọn chứa phương thức `.set`, phương thức này phải chấp nhận một giá trị mới cho `store` và gọi tất cả các hàm đăng ký hiện tại đồng bộ.
`Store` như vậy được gọi là `writable store`.

- Tương Thích Với `RxJS Observables:`

Phương thức `.subscribe` cũng có thể trả về một đối tượng với phương thức `.unsubscribe`, thay vì trả về hàm hủy đăng ký trực tiếp.
Tuy nhiên, trừ khi `.subscribe` gọi hàm đăng ký đồng bộ (mà không được yêu cầu bởi Observable spec), `Svelte` sẽ coi giá trị của `store` là `undefined` cho đến khi nó làm vậy.

```svelte
function createCustomStore(initialValue) {
	let value = initialValue;
	const subscribers = new Set();

	return {
		subscribe(subscriber) {
			subscribers.add(subscriber);
			subscriber(value); // Call the subscriber immediately with the current value

			return () => {
				subscribers.delete(subscriber); // Return an unsubscribe function
			};
		},
		set(newValue) {
			value = newValue;
			subscribers.forEach(subscriber => subscriber(value)); // Notify all subscribers
		}
	};
}

// Usage
const count = createCustomStore(0);

count.subscribe(value => {
	console.log(`Current value: ${value}`);
});

count.set(1); // Logs: Current value: 1
```

### <script context="module"> trong Svelte

- Trong `Svelte`, `<script>` với thuộc tính `context="module"` chạy một lần khi `module` lần đầu được đánh giá, thay vì chạy cho mỗi `instance` của `component`. Các giá trị khai báo trong khối này có thể được truy cập từ `<script>` thông thường (và từ phần `markup` của `component`), nhưng không ngược lại.

- Bạn có thể `export` các biến và hàm từ khối này, và chúng sẽ trở thành các `exports` của `module` đã biên dịch.

- Ví dụ: Hàm `alertTotal` có thể được nhập vào từ `module Svelte`.

- Bạn không thể `export  default` từ khối `module`, vì `export` mặc định của `module` là chính `component`.

- Các biến khai báo trong khối `module` không `reactive`. Việc gán lại giá trị cho chúng sẽ không kích hoạt việc `render` lại mặc dù biến đó có thể được cập nhật. Đối với các giá trị được chia sẻ giữa nhiều `component`, bạn nên sử dụng `store`.

```svelte
<script context="module">
	let totalComponents = 0;

	// Hàm này có thể được export như sau:
	// `import Example, { alertTotal } from './Example.svelte'`
	export function alertTotal() {
		alert(totalComponents);
	}
</script>

<script>
	totalComponents += 1;
	console.log(`total number of times this component has been created: ${totalComponents}`);
</script>
```

### <style>

#### CSS Scoped và Toàn Cục Trong Svelte

Trong `Svelte`, `CSS` bên trong khối <style> sẽ được giới hạn cho `component` đó. Điều này được thực hiện bằng cách thêm một lớp vào các phần tử bị ảnh hưởng, lớp này dựa trên một `hash` của các kiểu của `component` (ví dụ: svelte-123xyz).

#### CSS Scoped

`CSS` bên trong <style> block sẽ chỉ ảnh hưởng đến các phần tử trong `component` đó:

```svelte
<style>
	p {
		/* Điều này chỉ ảnh hưởng đến các <p> trong component này */
		color: burlywood;
	}
</style>
<style>
	:global(body) {
		/* Điều này sẽ áp dụng cho <body> */
		margin: 0;
	}

	div :global(strong) {
		/* Điều này sẽ áp dụng cho tất cả các <strong> trong bất kỳ component nào,
		   nếu chúng nằm trong các phần tử <div> thuộc về component này */
		color: goldenrod;
	}

	p:global(.red) {
		/* Điều này sẽ áp dụng cho tất cả các <p> thuộc về component này với lớp red,
		   ngay cả khi class="red" không xuất hiện trong markup ban đầu, và được thêm vào
		   tại runtime. Điều này hữu ích khi lớp của phần tử được áp dụng động, chẳng hạn như
		   khi cập nhật thuộc tính classList của phần tử trực tiếp. */
	}
</style>
```

#### Keyframes Toàn Cục và <style> Tag Trong Svelte

- `Keyframes` Toàn Cục
  Nếu bạn muốn tạo các `keyframes` mà có thể truy cập toàn cục, bạn cần thêm tiền tố `-global-` vào tên `keyframe` của bạn. Phần `-global-` sẽ bị loại bỏ khi biên dịch, và `keyframe` sẽ được tham chiếu chỉ bằng tên `my-animation-name` ở nơi khác trong mã của bạn.

```svelte
<style>
	@keyframes -global-my-animation-name {
		/* mã keyframe ở đây */
	}
</style>
```

Tên `keyframe` với `-global-` sẽ được biên dịch thành `my-animation-name` và có thể được sử dụng toàn cục trong mã `CSS` của bạn.

`<style> Tag` Trong `Component`
Mỗi `component` nên chỉ có một `<style> tag` cấp cao nhất. Tuy nhiên, bạn có thể có `<style> tag` lồng ghép bên trong các phần tử khác hoặc các khối `logic`.

Thí Dụ:

```svelte
<div>
	<style>
		/* tag <style> này sẽ được chèn vào DOM như vậy */
		div {
			/* Điều này sẽ áp dụng cho tất cả các phần tử `<div>` trong DOM */
			color: red;
		}
	</style>
</div>
```

`<style> tag` lồng ghép sẽ được chèn vào `DOM` mà không có xử lý hay giới hạn phạm vi nào, nghĩa là `CSS` bên trong nó sẽ áp dụng cho toàn bộ `DOM`, không bị giới hạn bởi `scoping` của `component`.

- `Keyframes` Toàn Cục: Sử dụng tiền tố `-global-` để đảm bảo `keyframes` có thể được truy cập toàn cục sau khi biên dịch.
  Một `<style> Tag`: Nên có một `<style> tag` cấp cao nhất trong mỗi `component`. `<style> tag` lồng ghép sẽ không được xử lý và áp dụng toàn cục trong `DOM`.

## Basic markup

### Tags

- Thẻ Viết Thường: Như `<div>`, đại diện cho các phần tử `HTML` thông thường.

- Thẻ Viết Hoa: Như `<Widget>` hoặc `<Namespace.Widget>`, chỉ định một `component`.

* HTML Element:

  ```svelte
  <div>
  	<!-- Đây là một phần tử HTML -->
  </div>
  ```

* Component:

  ```svelte
  <Widget>
  	<!-- Đây là một component -->
  </Widget>
  ```

* Component Với Namespace:

  ```svelte
  <Namespace.Widget>
  	<!-- Đây là một component trong namespace -->
  </Namespace.Widget>
  ```

### Attributes and props

#### Thuộc Tính Mặc Định

- Hoạt Động Giống `HTML`: Các thuộc tính hoạt động giống như các thuộc tính `HTML`.

```svelte
<div class="foo">
	<button disabled>can't touch this</button>
</div>
```

- Giá Trị Có Thể Không Có Dấu Nháy: Giá trị có thể không có dấu nháy.

```svelte
<!-- Không có dâú nháy -->
<input type="checkbox" />
```

- Biểu Thức `JavaScript`: Giá trị thuộc tính có thể chứa các biểu thức `JavaScript`.

```svelte
<a href="page/{p}">page {p}</a>
<button disabled={!clickable}>...</button>
```

- Thuộc Tính `Boolean`: Thuộc tính `boolean` được bao gồm nếu giá trị của nó là `truthy` và bị loại trừ nếu giá trị là `falsy`.

```svelte
<input required={false} placeholder="This input field is not required" />
<div title={null}>This div has no title attribute</div>
```

- Có Thể Được Đặt Trong Dấu Nháy: Nếu biểu thức có các ký tự gây lỗi cú pháp trong HTML thông thường, có thể sử dụng dấu nháy để bao quanh giá trị.

```svelte
<button disabled={number !== 42}>...</button>
```

- Rút Gọn Tên Thuộc Tính: Khi tên thuộc tính và giá trị khớp nhau `(name={name})`, có thể sử dụng cú pháp rút gọn.

```svelte
<button {disabled}>...</button>
<!-- Tương đương với <button disabled={disabled}>...</button> -->
```

#### Thuộc Tính Component

- `Thuộc Tính/Props` Trong `Component`: Các giá trị truyền vào `component` được gọi là `properties` hoặc `props`, không phải là thuộc tính `DOM`.

```svelte
<Widget foo={bar} answer={42} text="hello" />
```

- `Spread Attributes`: Cho phép truyền nhiều thuộc tính hoặc `properties` đến một phần tử hoặc `component` cùng một lúc. Một phần tử hoặc `component` có thể có nhiều `spread attributes`, xen kẽ với các thuộc tính thông thường.

```svelte
<Widget {...things} />
```

- `$$props`: Đại diện cho tất cả các `props` được truyền vào một `component`, bao gồm cả những `props` không được khai báo với `export`. `$$props` có thể không hiệu quả như việc tham chiếu một `prop` cụ thể vì mọi thay đổi ở bất kỳ `prop` nào đều gây ra việc `Svelte` phải kiểm tra lại tất cả các sử dụng của `$$props`.

```svelte
<Widget {...$$props} />
```

- `$$restProps`: Chứa các `props` không được khai báo với `export`. Có thể dùng để truyền các thuộc tính không xác định khác đến một phần tử trong một `component`. Hiệu suất tương đương như việc truy cập thuộc tính cụ thể.

```svelte
<input {...$$restProps} />
```

#### Thứ Tự Thuộc Tính

- Thứ Tự Quan Trọng: Đôi khi thứ tự thuộc tính quan trọng vì `Svelte` thiết lập thuộc tính tuần tự trong `JavaScript`. Ví dụ:

```svelte
<input type="range" min="0" max="1" value={0.5} step="0.1" />
<!-- Nên đổi thành <input type="range" min="0" max="1" step="0.1" value={0.5}/> -->
<img src="..." loading="lazy" />
<!-- Nên đổi thành <img loading="lazy" src="..."> -->
```

Giải Thích: `Svelte` có thể thiết lập thuộc tính theo thứ tự không mong muốn nếu bạn không chú ý đến thứ tự. Điều chỉnh thứ tự thuộc tính giúp đảm bảo rằng các thuộc tính được thiết lập đúng cách.

### Text expressions

- Trong `Svelte`, bạn có thể bao gồm các biểu thức `JavaScript` trong văn bản bằng cách đặt chúng trong dấu ngoặc nhọn.

- Biểu Thức JavaScript: Đặt biểu thức JavaScript vào dấu ngoặc nhọn để bao gồm nó trong template.

- Biểu Thức `RegExp`: Nếu bạn đang sử dụng cú pháp `literal` cho biểu thức chính quy `(RegExp)`, bạn cần bao nó trong dấu ngoặc đơn.

- Nếu bạn cần bao gồm dấu ngoặc nhọn { hoặc } trong `template` mà không phải là một biểu thức `JavaScript`, bạn có thể sử dụng các chuỗi thực thể `HTML`:

* Dấu ngoặc mở {:
  &lbrace;
  &lcub;
  &#123;
* Dấu ngoặc đóng }:
  &rbrace;
  &rcub;
  &#125;

```svelte
<h1>Hello {name}!</h1><p>{a} + {b} = {a + b}.</p>
<div>{/^[A-Za-z ]+$/.test(value) ? x : y}</div>
```

### Comments

Bạn có thể sử dụng các chú thích `HTML` bên trong các `component` của `Svelte` để thêm ghi chú hoặc tắt các cảnh báo.

- Chú Thích `HTML` Bình Thường:

```svelte
<!-- this is a comment! --><h1>Hello world</h1>
```

- Chú Thích `svelte-ignore`:

```svelte
<!-- svelte-ignore a11y-autofocus -->
<input bind:value={name} autofocus />
```

## Logic blocks

### {#if ...}

- Câu Lệnh `if` Đơn Giản: Bạn có thể dùng cú pháp `if` để chỉ hiển thị nội dung khi điều kiện là đúng.

```svelte
{#if expression}...{/if}
```

- Câu Lệnh `if` Với Điều Kiện Phụ: Bạn có thể thêm điều kiện phụ với `{:else if expression}`, và kết thúc bằng `{:else}` nếu cần.

```svelte
{#if expression}...{:else if expression}...{/if}
```

- Khối `if` Có Thể Bao Bọc Văn Bản: Các khối `if` không nhất thiết phải bao bọc các phần tử `HTML`. Chúng cũng có thể bao bọc văn bản trong các phần tử `HTML`.

```svelte
<p>
	{#if porridge.temperature > 100}
		Too hot!
	{:else if 80 > porridge.temperature}
		Too cold!
	{:else}
		Just right!
	{/if}
</p>
```

### {#each ...}

```svelte
{#each expression as name}...{/each}
```

```svelte
{#each expression as name, index}...{/each}
```

```svelte
{#each expression as name (key)}...{/each}
```

```svelte
{#each expression as name, index (key)}...{/each}
```

```svelte
{#each expression as name}...{:else}...{/each}
```

Ví dụ về việc lặp qua danh sách:

```svelte
<h1>Shopping list</h1>
<ul>
	{#each items as item}
		<li>{item.name} x {item.qty}</li>
	{/each}
</ul>
```

- Khối `each` có thể lặp qua bất kỳ mảng hoặc giá trị tương tự như mảng — đó là, bất kỳ đối tượng nào có thuộc tính `length`.

- Bạn có thể chỉ định chỉ số (tương đương với tham số thứ hai trong hàm `array.map(...)`):

```svelte
{#each items as item, i}
	<li>{i + 1}: {item.name} x {item.qty}</li>
{/each}
```

- Nếu cung cấp biểu thức `key` — mà phải xác định duy nhất mỗi mục trong danh sách — `Svelte` sẽ sử dụng nó để phân biệt danh sách khi dữ liệu thay đổi, thay vì thêm hoặc xóa các mục ở cuối. `key` có thể là bất kỳ đối tượng nào, nhưng chuỗi và số được khuyến nghị vì chúng cho phép danh tính vẫn còn khi các đối tượng thay đổi.

```svelte
{#each items as item (item.id)}
	<li>{item.name} x {item.qty}</li>
{/each}

<!-- hoặc với giá trị chỉ số bổ sung -->
{#each items as item, i (item.id)}
	<li>{i + 1}: {item.name} x {item.qty}</li>
{/each}
```

- Bạn có thể tự do sử dụng các mẫu phân tích cấu trúc `(destructuring)` và mẫu phần còn lại `(rest patterns)` trong các khối `each`.

```svelte
{#each items as { id, name, qty }, i (id)}
	<li>{i + 1}: {name} x {qty}</li>
{/each}

{#each objects as { id, ...rest }}
	<li><span>{id}</span><MyComponent {...rest} /></li>
{/each}

{#each items as [id, ...rest]}
	<li><span>{id}</span><MyComponent values={rest} /></li>
{/each}
```

- Khối `each` cũng có thể có một khối `{:else}`, được hiển thị nếu danh sách trống.

```svelte
{#each todos as todo}
	<p>{todo.text}</p>
{:else}
	<p>No tasks today!</p>
{/each}
```

- Từ `Svelte 4`, bạn có thể lặp qua các `iterable` như `Map` hoặc `Set`. Các `iterable` cần phải là hữu hạn và tĩnh (chúng không nên thay đổi trong khi đang lặp qua). Dưới đây, chúng sẽ được chuyển đổi thành mảng bằng `Array`.`from` trước khi được truyền cho quá trình `render`. Nếu bạn viết mã nhạy cảm với hiệu suất, hãy cố gắng tránh sử dụng `iterable` và sử dụng mảng thông thường vì chúng hiệu quả hơn.

### {#await ...}

```svelte
{#await expression}...{:then name}...{:catch name}...{/await}
```

```svelte
{#await expression}...{:then name}...{/await}
```

```svelte
{#await expression then name}...{/await}
```

```svelte
{#await expression catch name}...{/await}
```

Khối `await` trong `Svelte` cho phép bạn xử lý ba trạng thái khác nhau của một `Promise` — chờ `(pending)`, hoàn thành `(fulfilled)`, hoặc từ chối `(rejected)`. Trong chế độ `SSR (Server-Side Rendering)`, chỉ có nhánh chờ sẽ được `render` trên `server`. Nếu biểu thức được cung cấp không phải là một `Promise`, chỉ có nhánh hoàn thành sẽ được `render`, kể cả trong chế độ `SSR`.

- Cú pháp cơ bản:

```svelte
{#await expression}
	<!-- promise is pending -->
	<p>waiting for the promise to resolve...</p>
{:then name}
	<!-- promise was fulfilled or not a Promise -->
	<p>The value is {name}</p>
{:catch name}
	<!-- promise was rejected -->
	<p>Something went wrong: {name.message}</p>
{/await}
```

- Bỏ qua khối `catch` nếu không cần xử lý khi `promise` bị từ chối (hoặc không có lỗi):

```svelte
{#await promise}
	<!-- promise is pending -->
	<p>waiting for the promise to resolve...</p>
{:then value}
	<!-- promise was fulfilled -->
	<p>The value is {value}</p>
{/await}
```

- Nếu bạn không quan tâm đến trạng thái chờ, có thể bỏ qua khối này:

```svelte
{#await promise then value}
	<p>The value is {value}</p>
{/await}
```

- Tương tự, nếu bạn chỉ muốn hiển thị trạng thái lỗi, bạn có thể bỏ qua khối then:

```svelte
{#await promise catch error}
	<p>The error is {error}</p>
{/await}
```

### {#key ...}

```svelte
{#key expression}...{/key}
```

- Khối `key` trong `Svelte` giúp bạn kiểm soát việc hủy và tái tạo nội dung của một phần tử khi giá trị của biểu thức thay đổi. Điều này rất hữu ích khi bạn muốn phần tử hoặc `component` thực hiện một hiệu ứng chuyển tiếp `(transition)` mỗi khi giá trị thay đổi.

- Khi sử dụng với phần tử:

```svelte
{#key value}
	<div transition:fade>{value}</div>
{/key}
```

- Khi sử dụng với `component`:

```svelte
{#key value}
	<Component />
{/key}
```
