# learn-svelte

Tôi sử dụng repo này để theo dõi tất cả các bài học tôi đã học về `svelte`

# Getting Started

## Tài liệu tham khảo

- https://svelte.dev/docs/introduction

- https://learn.svelte.dev/tutorial/welcome-to-svelte

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

# Template Syntax

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

## Special tags

### {@html ...}

```svelte
{@html expression}
```

- Trong `Svelte`, các ký tự như `<` và `>` trong biểu thức văn bản sẽ được thoát `(escaped)`, có nghĩa là chúng sẽ không được hiển thị như là `HTML` mà chỉ là văn bản thông thường. Tuy nhiên, với các biểu thức `HTML`, chúng không bị thoát mà sẽ được hiển thị như là `HTML`.

- Biểu thức `{@html}` trong `Svelte` cho phép bạn chèn `HTML` trực tiếp vào trong `DOM`. Tuy nhiên, nó yêu cầu biểu thức phải là `HTML` hợp lệ. Ví dụ: `{@html "<div>"}` sẽ không hoạt động vì `<div>` chưa phải là `HTML` hợp lệ khi đứng một mình mà cần có thẻ đóng tương ứng.

- Đặc biệt, `Svelte` không tự động làm sạch `(sanitize)` các biểu thức trước khi chèn `HTML`. Nếu dữ liệu đến từ một nguồn không tin cậy, bạn phải làm sạch dữ liệu đó trước khi chèn nó vào `DOM`, nếu không, bạn sẽ mở ra lỗ hổng bảo mật `XSS (Cross-Site Scripting)`.

```svelte
<div class="blog-post">
	<h1>{post.title}</h1>
	{@html post.content}
</div>
```

### {@debug ...}

```svelte
{@debug}
```

```svelte
{@debug var1, var2, ..., varN}
```

Thẻ `{@debug ...}` cung cấp một giải pháp thay thế cho `console.log(...)`. Nó ghi lại giá trị của các biến cụ thể mỗi khi chúng thay đổi và tạm dừng việc thực thi mã nếu bạn mở công cụ phát triển `(devtools)`.

```svelte
<script>
	let user = {
		firstname: 'Ada',
		lastname: 'Lovelace'
	};
</script>

{@debug user}

<h1>Hello {user.firstname}!</h1>
```

Thẻ `{@debug ...}` chấp nhận danh sách tên biến được phân tách bằng dấu phẩy (không phải các biểu thức tùy ý).

```svelte
<!-- Có thể biên dịch -->
{@debug user}
{@debug user1, user2, user3}
<!-- Không thể biên dịch -->
{@debug user.firstname}
{@debug myArray[0]}
{@debug !isReady}
{@debug typeof user === 'object'}
```

Thẻ `{@debug}` không có đối số sẽ chèn một câu lệnh `debugger`, được kích hoạt khi bất kỳ trạng thái nào thay đổi, thay vì chỉ các biến cụ thể.

### {@const ...}

```svelte
{@const assignment}
```

Thẻ `{@const ...}` trong `Svelte` được sử dụng để định nghĩa một hằng số cục bộ.

```svelte
<script>
	export let boxes;
</script>

{#each boxes as box}
	{@const area = box.width * box.height}
	{box.width} * {box.height} = {area}
{/each}
```

Thẻ `{@const}` chỉ được phép sử dụng như là phần tử con trực tiếp của `{#if}`, `{:else if}`, `{:else}`, `{#each}`, `{:then}`, `{:catch}`, `<Component />` hoặc `<svelte:fragment />`.

## Element directives

Cũng như các thuộc tính, các phần tử có thể có các `directive`, giúp kiểm soát hành vi của phần tử theo một cách nào đó.

### on:eventname

```svelte
on:eventname={handler}
```

```svelte
on:eventname|modifiers={handler}
```

- Sử dụng directive on: để lắng nghe các sự kiện DOM.

```svelte
<script>
	let count = 0;

	/** @param {MouseEvent} event */
	function handleClick(event) {
		count += 1;
	}
</script>

<button on:click={handleClick}>
	count: {count}
</button>
```

- Các hàm xử lý sự kiện `(handlers)` có thể được khai báo trực tiếp mà không gây ra bất kỳ ảnh hưởng nào đến hiệu suất. Giống như các thuộc tính, giá trị của các `directive` có thể được đặt trong dấu ngoặc kép để hỗ trợ các công cụ tô sáng cú pháp.

```svelte
<button on:click={() => (count += 1)}>
	count: {count}
</button>
```

- Thêm các `modifier` vào sự kiện DOM bằng ký tự |.

```svelte
<form on:submit|preventDefault={handleSubmit}>
	<!-- Sự kiện `submit` sẽ bị ngăn chặn mặc định (preventDefault),
		 vì vậy trang sẽ không bị tải lại -->
</form>
```

- Các `modifier` sau đây có sẵn trong `Svelte`:

* `preventDefault` — gọi `event.preventDefault()` trước khi chạy hàm xử lý.
* `stopPropagation` — gọi `event.stopPropagation()`, ngăn sự kiện tiếp cận phần tử tiếp theo.
* `stopImmediatePropagation` — gọi `event.stopImmediatePropagation()`, ngăn các hàm xử lý khác của cùng một sự kiện không được kích hoạt.
* `passive` — cải thiện hiệu suất cuộn cho các sự kiện cảm ứng/cuộn (`Svelte` sẽ tự động thêm nó khi an toàn để làm như vậy).
* `nonpassive` — thiết lập rõ ràng `passive: false`.
* `capture` — kích hoạt hàm xử lý trong giai đoạn bắt đầu `(capture phase)` thay vì giai đoạn nổi bọt `(bubbling phase)`.
* `once` — loại bỏ hàm xử lý sau lần đầu tiên nó chạy.
* `self` — chỉ kích hoạt hàm xử lý nếu `event.target` là chính phần tử đó.
* `trusted` — chỉ kích hoạt hàm xử lý nếu `event.isTrusted` là `true`, tức là sự kiện được kích hoạt bởi hành động của người dùng.

- Các `modifier` có thể được kết hợp với nhau, ví dụ như `on:click|once|capture={...}`.

- Nếu `directive on`: được sử dụng mà không có giá trị, `component` sẽ chuyển tiếp sự kiện, có nghĩa là người sử dụng `component` có thể lắng nghe sự kiện đó.

```svelte
<button on:click>
	Trong ví dụ này, khi sự kiện click xảy ra trên nút, component sẽ phát ra sự kiện đó, cho phép các
	thành phần sử dụng component này lắng nghe và xử lý sự kiện click.
</button>
```

- Có thể có nhiều trình xử lý sự kiện cho cùng một sự kiện:

```svelte
<script>
	let counter = 0;
	function increment() {
		counter = counter + 1;
	}

	/** @param {MouseEvent} event */
	function track(event) {
		trackEvent(event);
	}
</script>

<!--  Khi người dùng nhấp vào nút, cả hai hàm increment và track sẽ được gọi -->
<button on:click={increment} on:click={track}>Click me!</button>
```

### bind:property

```svelte
bind:property={variable}
```

Dữ liệu thường chảy từ trên xuống, từ cha đến con. `Directive bind:` cho phép dữ liệu chảy theo hướng ngược lại, từ con lên cha. Hầu hết các liên kết `(bindings)` cụ thể cho các phần tử.

- Các liên kết đơn giản nhất phản ánh giá trị của một thuộc tính, chẳng hạn như `input.value`.

```svelte
<input bind:value={name} />
<textarea bind:value={text} />

<input type="checkbox" bind:checked={yes} />
```

- Nếu tên và giá trị trùng khớp, bạn có thể sử dụng cú pháp rút gọn.

```svelte
<input bind:value />
<!-- tương đương với
<input bind:value={value} />
-->
```

- Liên kết số: Các giá trị đầu vào số sẽ được chuyển đổi kiểu; mặc dù `input.value` là một chuỗi theo cách mà `DOM` xử lý, nhưng `Svelte` sẽ coi nó là một số. Nếu trường đầu vào trống hoặc không hợp lệ (trong trường hợp `type="number"`), giá trị sẽ là `null`.

```svelte
<input type="number" bind:value={num} />
<input type="range" bind:value={num} />
```

- Liên kết tệp: Với các phần tử `<input>` có thuộc tính `type="file"`, bạn có thể sử dụng `bind:files` để lấy `FileList` của các tệp đã chọn. Đây là một thuộc tính chỉ đọc.

```svelte
<label for="avatar">Upload a picture:</label>
<input accept="image/png, image/jpeg" bind:files id="avatar" name="avatar" type="file" />
```

- Thứ tự của các `directive`: Khi sử dụng các `directive bind:` cùng với các `directive on:`, thứ tự mà chúng được định nghĩa sẽ ảnh hưởng đến giá trị của biến liên kết khi hàm xử lý sự kiện được gọi.

```svelte
<script>
	let value = 'Hello World';
</script>

<input
	on:input={() => console.log('Old value:', value)}
	bind:value
	on:input={() => console.log('New value:', value)}
/>
```

Ở đây, chúng ta đang liên kết với giá trị của một trường đầu vào văn bản, sử dụng sự kiện `input`. Các liên kết trên các phần tử khác có thể sử dụng các sự kiện khác như `change`.

### Binding select value

- Liên kết giá trị của một phần tử `<select>` tương ứng với thuộc tính `value` của thẻ `<option>` được chọn, giá trị này có thể là bất kỳ kiểu dữ liệu nào (không chỉ là chuỗi như thường thấy trong `DOM`).

```svelte
<select bind:value={selected}>
	<option value={a}>a</option>
	<option value={b}>b</option>
	<option value={c}>c</option>
</select>
```

- Đối với phần tử `<select multiple>`, nó hoạt động tương tự như một nhóm `checkbox`. Biến liên kết là một mảng chứa các giá trị tương ứng với thuộc tính `value` của từng thẻ `<option>` được chọn.

```svelte
<select multiple bind:value={fillings}>
	<option value="Rice">Rice</option>
	<option value="Beans">Beans</option>
	<option value="Cheese">Cheese</option>
	<option value="Guac (extra)">Guac (extra)</option>
</select>
```

- Khi giá trị của một thẻ `<option>` khớp với nội dung văn bản của nó, bạn có thể bỏ qua thuộc tính `value`.

```svelte
<select multiple bind:value={fillings}>
	<option>Rice</option>
	<option>Beans</option>
	<option>Cheese</option>
	<option>Guac (extra)</option>
</select>
```

- Các phần tử có thuộc tính `contenteditable` hỗ trợ các liên kết sau:

* `innerHTML`
* `innerText`
* `textContent`

```svelte
<div contenteditable="true" bind:innerHTML={html} />
```

- Liên kết với phần tử `<details>`

```svelte
<details bind:open={isOpen}>
	<summary>Details</summary>
	<p>Something small enough to escape casual notice.</p>
</details>
```

### Media element bindings

- Bảy liên kết chỉ đọc `(readonly)`:

* `duration` (chỉ đọc) — tổng thời lượng của `video`, tính bằng giây.
* `buffered` (chỉ đọc) — một mảng các đối tượng `{start, end}` đại diện cho các khoảng đã tải trước.
* `played` (chỉ đọc) — tương tự như `buffered`, nhưng đại diện cho các khoảng đã phát.
* `seekable` (chỉ đọc) — tương tự như `buffered`, nhưng đại diện cho các khoảng có thể tua.
* `seeking` (chỉ đọc) — `boolean`, xác định xem `video` đang được tua hay không.
* `ended` (chỉ đọc) — `boolean`, xác định xem `video` đã kết thúc hay chưa.
* `readyState` (chỉ đọc) — một giá trị số từ 0 đến 4, biểu thị trạng thái sẵn sàng của `media`.

- Năm liên kết hai chiều:

* `currentTime` — thời gian phát hiện tại của `video`, tính bằng giây.
* `playbackRate` — tốc độ phát `video`, với giá trị 1 là tốc độ 'bình thường'.
* `paused` — `boolean`, xác định xem `video` có đang tạm dừng hay không.
* `volume` — giá trị từ 0 đến 1, biểu thị âm lượng của `video`.
* `muted` — `boolean`, xác định xem `video` có bị tắt tiếng hay không.

- Liên kết bổ sung cho `video`:

* `videoWidth` (chỉ đọc) — chiều rộng của `video`.
* `videoHeight` (chỉ đọc) — chiều cao của `video`.

```svelte
<video
	src={clip}
	bind:duration
	bind:buffered
	bind:played
	bind:seekable
	bind:seeking
	bind:ended
	bind:readyState
	bind:currentTime
	bind:playbackRate
	bind:paused
	bind:volume
	bind:muted
	bind:videoWidth
	bind:videoHeight
/>
```

### Image element bindings

- Phần tử `<img>` có hai liên kết chỉ đọc:

* `naturalWidth` (chỉ đọc) — chiều rộng gốc của hình ảnh, có sẵn sau khi hình ảnh đã được tải.
* `naturalHeight` (chỉ đọc) — chiều cao gốc của hình ảnh, có sẵn sau khi hình ảnh đã được tải.

```svelte
<img
	bind:naturalWidth
	bind:naturalHeight
></img>
```

### Block-level element bindings

- Các phần tử cấp khối có bốn liên kết chỉ đọc, được đo bằng kỹ thuật tương tự như sau:

* `clientWidth` (chỉ đọc) — chiều rộng của nội dung phần tử, bao gồm `padding` nhưng không bao gồm viền và cuộn dọc.
* `clientHeight` (chỉ đọc) — chiều cao của nội dung phần tử, bao gồm `padding` nhưng không bao gồm viền và cuộn dọc.
* `offsetWidth` (chỉ đọc) — chiều rộng của phần tử, bao gồm `padding`, viền và cuộn dọc.
* `offsetHeight` (chỉ đọc) — chiều cao của phần tử, bao gồm `padding`, viền và cuộn dọc.

```svelte
<div bind:offsetWidth={width} bind:offsetHeight={height}>
	<Chart {width} {height} />
</div>
```

### bind:group

```svelte
bind:group={variable}
```

- Bạn có thể sử dụng `bind:group` để liên kết các `input` hoạt động cùng nhau.

```svelte
<script>
	let tortilla = 'Plain';

	/** @type {Array<string>} */
	let fillings = [];
</script>

<!-- Các radio inputs trong cùng một nhóm sẽ loại trừ lẫn nhau -->
<input type="radio" bind:group={tortilla} value="Plain" />
<input type="radio" bind:group={tortilla} value="Whole wheat" />
<input type="radio" bind:group={tortilla} value="Spinach" />

<!-- Các checkbox inputs trong cùng một nhóm sẽ thêm giá trị vào một mảng -->
<input type="checkbox" bind:group={fillings} value="Rice" />
<input type="checkbox" bind:group={fillings} value="Beans" />
<input type="checkbox" bind:group={fillings} value="Cheese" />
<input type="checkbox" bind:group={fillings} value="Guac (extra)" />
```

- Chú thích:

* `Radio inputs`: Các `radio inputs` trong cùng một nhóm `bind:group` sẽ loại trừ lẫn nhau, tức là chỉ một lựa chọn có thể được chọn tại một thời điểm.

* `Checkbox inputs`: Các `checkbox inputs` trong cùng một nhóm `bind:group` sẽ thêm giá trị vào một mảng. Mỗi `checkbox` được chọn sẽ được thêm vào mảng `fillings`.

- Lưu ý:
  `bind:group` chỉ hoạt động nếu các `input` nằm trong cùng một thành phần `Svelte`.

### bind:this

Bạn có thể sử dụng `bind:this` để lấy tham chiếu đến một `DOM` node cụ thể.

```svelte
<script>
	import { onMount } from 'svelte';

	/** @type {HTMLCanvasElement} */
	let canvasElement;

	onMount(() => {
		const ctx = canvasElement.getContext('2d');
		drawStuff(ctx);
	});
</script>

<canvas bind:this={canvasElement} />
```

### class:name

```svelte
class:name={value}
```

```svelte
class:name
```

- Sử dụng `class`: để thao tác với lớp `CSS`

```svelte
<!-- Các ví dụ này là tương đương -->
<div class={isActive ? 'active' : ''}>...</div>
<div class:active={isActive}>...</div>

<!-- Viết tắt khi tên lớp và giá trị đều giống nhau -->
<div class:active>...</div>

<!-- Có thể bao gồm nhiều chỉ thị class để thao tác với nhiều lớp cùng lúc -->
<div class:active class:inactive={!active} class:isAdmin>...</div>
```

### style:property

```svelte
style:property={value}
```

```svelte
style:property="value"
```

```svelte
style:property
```

- Sử dụng `style`: để thiết lập nhiều thuộc tính `CSS` trên một phần tử

```svelte
<!-- Các ví dụ này là tương đương -->
<div style:color="red">...</div>
<div style="color: red;">...</div>

<!-- Có thể sử dụng biến để thiết lập giá trị CSS -->
<div style:color={myColor}>...</div>

<!-- Viết tắt khi tên thuộc tính và tên biến giống nhau -->
<div style:color>...</div>

<!-- Có thể thiết lập nhiều thuộc tính CSS cùng lúc -->
<div style:color style:width="12rem" style:background-color={darkMode ? 'black' : 'white'}>...</div>

<!-- Các thuộc tính CSS có thể được đánh dấu là quan trọng -->
<div style:color|important="red">...</div>
```

### use:action

```svelte
use:action
```

```svelte
use:action={parameters}
```

```svelte
action = (node: HTMLElement, parameters: any) => {
	update?: (parameters: any) => void,
	destroy?: () => void
}
```

- `Actions` là các hàm được gọi khi một phần tử được tạo ra trong `DOM`. Chúng có thể trả về một đối tượng với phương thức `destroy`, phương thức này sẽ được gọi khi phần tử bị xóa khỏi `DOM`.

```svelte
<script>
	/** @type {import('svelte/action').Action}  */
	function foo(node) {
		// phần tử `node` đã được gắn vào DOM

		return {
			destroy() {
				// phần tử `node` đã bị xóa khỏi DOM
			}
		};
	}
</script>

<div use:foo />
```

- Thêm tham số cho `Action`

* Một `Action` có thể nhận một tham số. Nếu giá trị trả về của hàm có phương thức `update`, phương thức này sẽ được gọi bất cứ khi nào tham số thay đổi, ngay sau khi `Svelte` cập nhật `markup`.

* Không cần lo lắng về việc hàm `foo` được khai báo lại cho mỗi `instance` của `component` — `Svelte` sẽ tự động đưa những hàm không phụ thuộc vào trạng thái cục bộ ra khỏi định nghĩa của `component`.

```svelte
<script>
	export let bar;

	/** @type {import('svelte/action').Action}  */
	function foo(node, bar) {
		// phần tử `node` đã được gắn vào DOM

		return {
			update(bar) {
				// giá trị của `bar` đã thay đổi
			},

			destroy() {
				// phần tử `node` đã bị xóa khỏi DOM
			}
		};
	}
</script>

<div use:foo={bar} />
```

- Tóm tắt:

* `Actions` là các hàm xử lý cho các phần tử `DOM`, có thể trả về các phương thức `update` và `destroy`.
* `update`: Được gọi khi tham số của `Action` thay đổi.
* `destroy`: Được gọi khi phần tử `DOM` bị xóa.

### transition:fn

```svelte
transition:fn
```

```svelte
transition:fn={params}
```

```svelte
transition:fn|global
```

```svelte
transition:fn|global={params}
```

```svelte
transition:fn|local
```

```svelte
transition:fn|local={params}
```

```svelte
transition = (node: HTMLElement, params: any, options: { direction: 'in' | 'out' | 'both' }) => {
	delay?: number,
	duration?: number,
	easing?: (t: number) => number,
	css?: (t: number, u: number) => string,
	tick?: (t: number, u: number) => void
}
```

- `Transition` được kích hoạt khi một phần tử xuất hiện hoặc biến mất khỏi `DOM` do thay đổi trạng thái.

* Khi một khối đang thực hiện quá trình chuyển đổi ra khỏi `DOM`, tất cả các phần tử bên trong khối đó, bao gồm cả những phần tử không có chuyển đổi riêng, sẽ được giữ lại trong `DOM` cho đến khi mọi chuyển đổi trong khối đó hoàn thành.
* Chỉ thị `transition`:
* Chỉ thị `transition`: xác định một quá trình chuyển đổi hai chiều, nghĩa là nó có thể được đảo ngược một cách mượt mà trong khi chuyển đổi đang diễn ra.

```svelte
{#if visible}
	<div transition:fade>fades in and out</div>
{/if}
```

- `Transitions` cục bộ và toàn cục

* `Transitions` cục bộ `(local)`: Mặc định, trong `Svelte 3`, `transitions` là toàn cục, nhưng hiện tại chúng là cục bộ. Chúng chỉ được kích hoạt khi khối chứa chúng được tạo hoặc bị hủy, không phải khi các khối cha được tạo hoặc hủy.
* `Transitions` toàn cục `(global)`: Nếu muốn một 1 được kích hoạt cả khi khối cha thay đổi, bạn có thể sử dụng `|global`.

```svelte
{#if x}
	{#if y}
		<p transition:fade>fades in and out only when y changes</p>

		<p transition:fade|global>fades in and out when x or y change</p>
	{/if}
{/if}
```

- Chuyển đổi `intro` khi `render` lần đầu

* Mặc định, các chuyển đổi `intro` sẽ không chạy khi `render` lần đầu tiên. Bạn có thể thay đổi hành vi này bằng cách đặt `intro: true` khi tạo `component` và đánh dấu `transition` là `global`.

```svelte
<YourComponent intro={true} transition:fade|global />
```

### Transition parameters

- Sử dụng Tham số với `Transitions` trong `Svelte`

- Tương tự như `actions`, `transitions` trong `Svelte` cũng có thể nhận các tham số.

Các tham số này được truyền dưới dạng một đối tượng trong một tag biểu thức, sử dụng cú pháp `{{}}`.

```svelte
{#if visible}
	<div transition:fade={{ duration: 2000 }}>fades in and out over two seconds</div>
{/if}
```

### Custom transition functions

- Sử dụng Hàm Tùy Chỉnh với `Transitions` trong `Svelte`

* `Transitions` trong `Svelte` có thể sử dụng các hàm tùy chỉnh để tạo hiệu ứng chuyển đổi linh hoạt hơn. Khi sử dụng hàm tùy chỉnh, nếu đối tượng trả về có thuộc tính `css`, `Svelte` sẽ tạo một hoạt ảnh `CSS` cho phần tử.

- Hàm `whoosh`:

```svelte
<script>
	import { elasticOut } from 'svelte/easing';

	/** @type {boolean} */
	export let visible;

	/**
	 * @param {HTMLElement} node
	 * @param {{ delay?: number, duration?: number, easing?: (t: number) => number }} params
	 */
	function whoosh(node, params) {
		const existingTransform = getComputedStyle(node).transform.replace('none', '');

		return {
			delay: params.delay || 0,
			duration: params.duration || 400,
			easing: params.easing || elasticOut,
			css: (t, u) => `transform: ${existingTransform} scale(${t})`
		};
	}
</script>

{#if visible}
	<div in:whoosh>whooshes in</div>
{/if}
```

- Hàm Tùy Chỉnh với `tick`, `typewriter`

```svelte
<script>
	export let visible = false;

	/**
	 * @param {HTMLElement} node
	 * @param {{ speed?: number }} params
	 */
	function typewriter(node, { speed = 1 }) {
		const valid = node.childNodes.length === 1 && node.childNodes[0].nodeType === Node.TEXT_NODE;

		if (!valid) {
			throw new Error(`This transition only works on elements with a single text node child`);
		}

		const text = node.textContent;
		const duration = text.length / (speed * 0.01);

		return {
			duration,
			tick: (t) => {
				const i = ~~(text.length * t);
				node.textContent = text.slice(0, i);
			}
		};
	}
</script>

{#if visible}
	<p in:typewriter={{ speed: 1 }}>The quick brown fox jumps over the lazy dog</p>
{/if}
```

- Phối hợp Các `Transition`

* Nếu một hàm chuyển đổi trả về một hàm thay vì một đối tượng chuyển đổi, hàm này sẽ được gọi trong `microtask` tiếp theo. Điều này cho phép phối hợp nhiều chuyển đổi, tạo ra hiệu ứng như `crossfade`.

- Tham số `options`:

* `direction` là một trong các giá trị: `in`, `out`, hoặc `both`, tùy thuộc vào loại chuyển đổi.
* Sử dụng các hàm tùy chỉnh cho phép bạn tạo ra các hiệu ứng chuyển đổi tinh vi và sáng tạo trong ứng dụng `Svelte` của bạn.

### Transition events

- Khi một phần tử có hiệu ứng chuyển đổi `(transitions)`, nó sẽ phát ra các sự kiện bổ sung ngoài các sự kiện DOM tiêu chuẩn. Các sự kiện này giúp bạn theo dõi và điều khiển quá trình chuyển đổi.

- Các Sự Kiện Chuyển Đổi:

* `introstart`: Kích hoạt khi chuyển đổi vào bắt đầu.
* `introend`: Kích hoạt khi chuyển đổi vào kết thúc.
* `outrostart`: Kích hoạt khi chuyển đổi ra bắt đầu.
* `outroend`: Kích hoạt khi chuyển đổi ra kết thúc.

```svelte
{#if visible}
	<p
		transition:fly={{ y: 200, duration: 2000 }}
		on:introstart={() => (status = 'intro started')}
		on:outrostart={() => (status = 'outro started')}
		on:introend={() => (status = 'intro ended')}
		on:outroend={() => (status = 'outro ended')}
	>
		Flies in and out
	</p>
{/if}
```

### in:fn/out:fn

```svelte
in:fn
```

```svelte
in:fn={params}
```

```svelte
in:fn|global
```

```svelte
in:fn|global={params}
```

```svelte
in:fn|local
```

```svelte
in:fn|local={params}
```

```svelte
out:fn
```

```svelte
out:fn={params}
```

```svelte
out:fn|global
```

```svelte
out:fn|global={params}
```

```svelte
out:fn|local
```

```svelte
out:fn|local={params}
```

- Khi bạn muốn áp dụng hiệu ứng chuyển đổi cho các phần tử khi chúng vào `(in:)` hoặc rời khỏi `(out:)` `DOM`, bạn có thể sử dụng các chỉ thị `in:` và `out:.`

- Khác Biệt Với `transition`:

* `in::` Áp dụng hiệu ứng khi phần tử vào `DOM`.
* `out::` Áp dụng hiệu ứng khi phần tử rời khỏi `DOM`.
* Các `transitions` áp dụng với `in:` và `out:` không phải là hai chiều như với `transition:`. Điều này có nghĩa là:

- Một `transition in:` sẽ không đảo ngược nếu một `transition out:` đang diễn ra. Thay vào đó, `transition in:` sẽ tiếp tục khi `transition out:` đang được thực hiện.
  Nếu một `transition out: `bị hủy, `transitions` sẽ bắt đầu lại từ đầu.

### animate:fn

```svelte
animate:name
```

```svelte
animate:name
```

```svelte
animation = (node: HTMLElement, { from: DOMRect, to: DOMRect } , params: any) => {
	delay?: number,
	duration?: number,
	easing?: (t: number) => number,
	css?: (t: number, u: number) => string,
	tick?: (t: number, u: number) => void
}
```

```svelte
DOMRect {
	bottom: number,
	height: number,
	​​left: number,
	right: number,
	​top: number,
	width: number,
	x: number,
	y: number
}
```

- Sử Dụng `Animation` Trong Các Khối `each` Có Khóa

* Khi nội dung của một khối `each` có khóa `(keyed each)` được sắp xếp lại, một hiệu ứng hoạt hình `(animation)` sẽ được kích hoạt. Các hiệu ứng hoạt hình không chạy khi phần tử được thêm vào hoặc xóa khỏi khối, mà chỉ khi chỉ số của một mục dữ liệu hiện tại trong khối thay đổi.

- Điều Kiện:

* Khóa `(Keyed)`: Để `animation` hoạt động khi các mục được sắp xếp lại, khối each phải sử dụng khóa (`(item)` trong ví dụ).
  Phần Tử Con Ngay Lập Tức: Các chỉ thị `animate`: phải được đặt trên phần tử là con ngay lập tức của khối `each` có khóa.

```svelte
<!-- Khi `list` được sắp xếp lại, hiệu ứng hoạt hình sẽ chạy -->
{#each list as item, index (item)}
	<li animate:flip>{item}</li>
{/each}
```

### Animation Parameters

- Tham Số Của `Animation`

* Giống như với `actions` và `transitions`, `animations` cũng có thể nhận các tham số.

- Bạn có thể truyền các tham số cho hiệu ứng hoạt hình bằng cách sử dụng cú pháp đối tượng trong dấu ngoặc nhọn `(double curly braces)`.

```svelte
{#each list as item, index (item)}
	<li animate:flip={{ delay: 500 }}>{item}</li>
{/each}
```

### Custom animation functions

`Animations` trong `Svelte` có thể sử dụng các hàm tùy chỉnh để điều khiển cách hoạt động của chúng. Các hàm này nhận đối số là node, một đối tượng `animation`, và các tham số khác, để tạo ra các hiệu ứng `animation` đặc biệt.

- Ví Dụ: Hàm `Animation` Tùy Chỉnh

```svelte
<script>
	import { cubicOut } from 'svelte/easing';

	/**
	 * @param {HTMLElement} node
	 * @param {{ from: DOMRect; to: DOMRect }} states
	 * @param {any} params
	 */
	function whizz(node, { from, to }, params) {
		const dx = from.left - to.left;
		const dy = from.top - to.top;

		const d = Math.sqrt(dx * dx + dy * dy);

		return {
			delay: 0,
			duration: Math.sqrt(d) * 120,
			easing: cubicOut,
			css: (t, u) => `transform: translate(${u * dx}px, ${u * dy}px) rotate(${t * 360}deg);`
		};
	}
</script>

{#each list as item, index (item)}
	<div animate:whizz>{item}</div>
{/each}
```

- Sử Dụng `tick` Thay Vì `css`

```svelte
<script>
	import { cubicOut } from 'svelte/easing';

	/**
	 * @param {HTMLElement} node
	 * @param {{ from: DOMRect; to: DOMRect }} states
	 * @param {any} params
	 */
	function whizz(node, { from, to }, params) {
		const dx = from.left - to.left;
		const dy = from.top - to.top;

		const d = Math.sqrt(dx * dx + dy * dy);

		return {
			delay: 0,
			duration: Math.sqrt(d) * 120,
			easing: cubicOut,
			tick: (t, u) => Object.assign(node.style, { color: t > 0.5 ? 'Pink' : 'Blue' })
		};
	}
</script>

{#each list as item, index (item)}
	<div animate:whizz>{item}</div>
{/each}
```

## Component directives

### on:eventname

```svelte
on:eventname={handler}
```

- Trong `Svelte`, các `component` có thể phát ra sự kiện bằng cách sử dụng `createEventDispatcher` hoặc chuyển tiếp các sự kiện `DOM`.

- Sử Dụng `createEventDispatcher`

* Bạn có thể sử dụng `createEventDispatcher` để phát tín hiệu một cách lập trình.

```svelte
<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();
</script>

<!-- Phát tín hiệu "hello" khi nút được nhấn -->
<button on:click={() => dispatch('hello')}> one </button>
```

- Chuyển Tiếp Sự Kiện `DOM`

* Nếu bạn muốn chuyển tiếp sự kiện `DOM` từ một `element` trong `component` của mình, bạn có thể sử dụng cú pháp đơn giản.

```svelte
<button on:click> two </button>
```

- Lắng Nghe Sự Kiện Từ `Component`

* Lắng nghe sự kiện từ một `component` hoạt động tương tự như lắng nghe các sự kiện `DOM`. Bạn chỉ cần sử dụng on: để chỉ định hàm xử lý.

```svelte
<SomeComponent on:whatever={handler} />
```

- `on:whatever={handler}`: Bất kỳ sự kiện nào phát từ `SomeComponent` với tên `'whatever'` sẽ được xử lý bởi hàm `handler`.

- Chuyển Tiếp Sự Kiện

* Nếu bạn sử dụng `on`: mà không kèm theo hàm xử lý (giá trị), sự kiện sẽ được chuyển tiếp, cho phép `component` cha lắng nghe sự kiện này.

```svelte
<SomeComponent on:whatever />
```

- Chuyển tiếp sự kiện: Với cú pháp này, sự kiện `'whatever'` sẽ được chuyển tiếp lên `component` cha của `SomeComponent`, và nó có thể tiếp tục được lắng nghe ở cấp cao hơn.

- Tổng Kết

- `createEventDispatcher`: Sử dụng để phát ra các sự kiện từ `component` một cách lập trình.

- Chuyển tiếp sự kiện `DOM`: Sử dụng cú pháp `on`: mà không kèm giá trị để chuyển tiếp sự kiện.

- Lắng nghe sự kiện: Bạn có thể lắng nghe các sự kiện từ `component` giống như lắng nghe sự kiện `DOM`.

- Chuyển tiếp sự kiện: Cho phép sự kiện được lắng nghe ở `component` cha hoặc cấp cao hơn.

### --style-props

```svelte
--style-props="anycssvalue"
```

Trong `Svelte`, bạn có thể truyền các `custom CSS properties` (các biến CSS) dưới dạng `props` vào `component` để tạo giao diện tùy biến. Điều này giúp bạn dễ dàng tạo các `component` có thể tùy biến giao diện dựa trên các chủ đề `(theme)` khác nhau.

- Khi bạn truyền các custom `properties` vào `component` như sau:

```svelte
<Slider bind:value min={0} --rail-color="black" --track-color="rgb(0, 0, 255)" />
```

- Svelte sẽ thực hiện việc `"desugar"` cú pháp này thành một phần tử bọc bên ngoài, như sau:

```svelte
<div style="display: contents; --rail-color: black; --track-color: rgb(0, 0, 255)">
	<Slider bind:value min={0} />
</div>
```

- Lưu Ý Khi Sử Dụng

* Do có thêm phần tử bọc `<div>`, bạn cần chú ý khi viết `CSS` vì có thể các quy tắc `CSS` của bạn vô tình nhắm mục tiêu vào phần tử bọc này thay vì phần tử thực sự bên trong.

* Sử Dụng Trong SVG
  Khi component được sử dụng trong `SVG`, cú pháp tương tự sẽ `"desugar"` thành một phần tử <g>:

```svelte
<g style="--rail-color: black; --track-color: rgb(0, 0, 255)">
	<Slider bind:value min={0} />
</g>
```

- Tương tự, bạn cũng cần chú ý đến các quy tắc `CSS` của bạn khi làm việc với `SVG`.

- Hỗ Trợ `CSS Variables` Trong `Svelte`
  `Svelte` hỗ trợ sử dụng các biến `CSS`, giúp tạo ra các `component` có thể dễ dàng tùy biến giao diện theo chủ đề. Ví dụ:

```svelte
<style>
	.potato-slider-rail {
		background-color: var(--rail-color, var(--theme-color, 'purple'));
	}
</style>
```

- Trong ví dụ này, biến `--rail-color` sẽ xác định màu nền của `rail`. Nếu `--rail-color` không được cung cấp, `--theme-color` sẽ được sử dụng làm giá trị mặc định. Nếu cả hai biến đều không được đặt, màu mặc định là `'purple'`.

- Cài Đặt Màu Sắc Chủ Đề Toàn Cục
  Bạn có thể đặt màu chủ đề ở cấp độ toàn cục:

```svelte
/* global.css */
html {
	--theme-color: black;
}
```

- Hoặc bạn có thể ghi đè nó ở cấp độ `component`:

```svelte
<Slider --rail-color="goldenrod" />
```

- `Svelte` cung cấp một cách tiện lợi để sử dụng `CSS custom properties`, giúp tạo ra các `component` có khả năng tùy biến cao và dễ dàng tích hợp với các hệ thống chủ đề `(theming)`. Tuy nhiên, bạn cần lưu ý về các phần tử bọc được thêm vào khi sử dụng tính năng này, đặc biệt là khi viết `CSS`.

### bind:property

```svelte
bind:property={variable}
```

- Trong `Svelte`, bạn có thể bind các `props` của `component` bằng cách sử dụng cú pháp tương tự như khi bind các thuộc tính của phần tử `HTML`.

```svelte
<Keypad bind:value={pin} />
```

- `bind:value={pin}`: Kết nối `(bind) prop value` của `component Keypad` với biến `pin` trong `parent component`. Điều này có nghĩa là mọi thay đổi của `pin` trong `parent` sẽ ảnh hưởng đến `prop value` của `Keypad`, và ngược lại, nếu `value` thay đổi trong `Keypad`, nó sẽ cập nhật `pin`.

- `Binding props` trong `Svelte` sử dụng cú pháp `bind:property={variable}` giúp dữ liệu và trạng thái có thể phản ứng `(reactive)` hai chiều giữa `component` cha và `component` con. Điều này tạo ra sự linh hoạt khi xây dựng các `component` tương tác và phức tạp.

### bind:this

```svelte
bind:this={component_instance}
```

- Các `component` cũng hỗ trợ `bind:this`, cho phép bạn tương tác với các `instance` của `component` một cách lập trình.

```svelte
<ShoppingCart bind:this={cart} />

<button on:click={() => cart.empty()}> Empty shopping cart </button>
```

- Lưu ý rằng chúng ta không thể làm `{cart.empty}` vì `cart` sẽ là `undefined` khi nút bấm được `render` lần đầu và sẽ gây ra lỗi.

# Runtime

## svelte

`Svelte package` cung cấp các hàm vòng đời và `API` ngữ cảnh `(context API)`.

### onMount

- Hàm `onMount` trong `Svelte` được sử dụng để lên lịch một `callback` chạy ngay khi `component` đã được gắn vào `DOM`. Hàm này phải được gọi trong quá trình khởi tạo của `component` (nhưng không nhất thiết phải nằm bên trong `component`; nó có thể được gọi từ một `module` bên ngoài).

```svelte
function onMount<T>(
	fn: () =>
		| NotFunction<T>
		| Promise<NotFunction<T>>
		| (() => any)
): void;
```
 + Hàm `onMount` sẽ chạy callback ngay sau khi `component` đã được gắn vào `DOM`. `Callback` này sẽ không chạy trong các `component` được `render` phía `server-side`.

 + Nếu một hàm được trả về từ `onMount`, nó sẽ được gọi khi `component` bị tháo ra khỏi `DOM`. Điều này thường được sử dụng để dọn dẹp các tài nguyên hoặc lắng nghe sự kiện khi `component` không còn cần thiết nữa.

- Chạy code khi `component` được gắn vào `DOM`:

```svelte
<script>
	import { onMount } from 'svelte';

	onMount(() => {
		console.log('the component has mounted');
	});
</script>
```
  + Trong ví dụ này, `console.log('the component has mounted')` sẽ chạy khi `component` được gắn vào `DOM`.

- Dọn dẹp khi `component` bị tháo ra khỏi `DOM`:

```svelte
<script>
	import { onMount } from 'svelte';

	onMount(() => {
		const interval = setInterval(() => {
			console.log('beep');
		}, 1000);

		// Dọn dẹp bằng cách trả về hàm clearInterval
		return () => clearInterval(interval);
	});
</script>
```

- Ở đây, `setInterval` tạo ra một `interval` chạy mỗi giây. Khi `component` bị tháo ra khỏi `DOM`, hàm trả về từ `onMount` sẽ được gọi để dọn dẹp `interval`.

- Lưu ý:

 + Hành vi này chỉ hoạt động khi hàm được truyền vào `onMount` đồng bộ trả về một giá trị. Các hàm `async` luôn trả về một `Promise`, do đó không thể đồng bộ trả về một hàm.

 ### beforeUpdatepermalink

 Hàm `beforeUpdate` trong `Svelte` được sử dụng để lên lịch một `callback` chạy ngay trước khi `component` được cập nhật sau bất kỳ thay đổi nào về trạng thái.

```svelte
function beforeUpdate(fn: () => any): void;
```

- Mô tả:

 + Hàm `beforeUpdate` cho phép bạn thực hiện các thao tác ngay trước khi `Svelte` cập nhật `DOM` để phản ánh các thay đổi về trạng thái của `component`.

 + Lần đầu tiên `callback` được chạy sẽ là trước khi `onMount` được gọi. Điều này có nghĩa là `beforeUpdate` có thể chạy ngay cả trước khi `component` thực sự được gắn vào `DOM`.

```svelte
<script>
	import { beforeUpdate } from 'svelte';

	beforeUpdate(() => {
		console.log('the component is about to update');
	});
</script>
```

Trong ví dụ này, `console.log('the component is about to update')` sẽ chạy mỗi khi `component` chuẩn bị được cập nhật sau một thay đổi về trạng thái. Nó cho phép bạn thực hiện các thao tác trước khi `DOM` của `component` được cập nhật.

### afterUpdatepermalink

- Hàm `afterUpdate` trong `Svelte` được sử dụng để lên lịch một `callback` chạy ngay sau khi `component` đã được cập nhật.

```svelte
function afterUpdate(fn: () => any): void;
```

- Mô tả:
 + Hàm `afterUpdate` cho phép bạn thực hiện các thao tác ngay sau khi `Svelte` đã cập nhật `DOM` để phản ánh các thay đổi về trạng thái của `component`.
 + Lần đầu tiên `callback` này chạy sẽ là sau khi `onMount` đã được thực thi.

 ```svelte
<script>
	import { afterUpdate } from 'svelte';

	afterUpdate(() => {
		console.log('the component just updated');
	});
</script>
```

### onDestroypermalink

```svelte
function onDestroy(fn: () => any): void;
```

- Hàm `onDestroy` trong `Svelte` được sử dụng để lên lịch một `callback` chạy ngay trước khi `component` bị tháo ra khỏi `DOM`.

- Mô tả:
Hàm `onDestroy` cho phép bạn thực hiện các thao tác dọn dẹp, chẳng hạn như hủy bỏ các sự kiện, xóa bộ hẹn giờ, hoặc giải phóng tài nguyên, ngay trước khi `component` bị phá hủy.
- Khác với `onMount`, `beforeUpdate`, và `afterUpdate`, hàm `onDestroy` cũng chạy trong các `component` được `render` phía `server-side`.

```svelte
<script>
	import { onDestroy } from 'svelte';

	onDestroy(() => {
		console.log('the component is being destroyed');
	});
</script>
```

- Trong ví dụ này, `console.log('the component is being destroyed')` sẽ chạy ngay trước khi `component` bị tháo ra khỏi `DOM`, giúp bạn xử lý bất kỳ công việc nào cần thiết khi `component` không còn tồn tại nữa.

### tick

```svelte
function tick(): Promise<void>;
```

- Hàm `tick` trong `Svelte` trả về một `Promise` và được sử dụng để đợi cho đến khi tất cả các thay đổi trạng thái đang chờ xử lý đã được áp dụng. Nếu không có thay đổi nào đang chờ, `tick` sẽ đợi đến `microtask` tiếp theo trước khi tiếp tục.

```svelte
import { tick } from 'svelte';

async function example() {
	await tick();
	// code to execute after state changes have been applied
}
```

- Mô tả:
 + Hàm `tick` cho phép bạn thực hiện một tác vụ ngay sau khi `Svelte` đã hoàn tất việc cập nhật `DOM` với các thay đổi trạng thái hiện tại.
 + Khi gọi `tick`, bạn có thể chờ cho đến khi mọi cập nhật về trạng thái đã được phản ánh trong `DOM` trước khi thực hiện bước tiếp theo trong logic của mình.

```svelte
<script>
	import { beforeUpdate, tick } from 'svelte';

	beforeUpdate(async () => {
		console.log('the component is about to update');
		await tick();
		console.log('the component just updated');
	});
</script>
```

- Trong ví dụ này:

 + `console.log('the component is about to update')` sẽ chạy ngay trước khi `component` được cập nhật.
 + `await tick()` sẽ đợi cho đến khi tất cả các thay đổi về trạng thái đã được áp dụng và DOM đã được cập nhật.
 + Sau khi `tick` hoàn tất, c`onsole.log('the component just updated')` sẽ chạy, cho biết rằng `component` đã được cập nhật xong.

 ### setContext

 - Hàm `setContext` trong `Svelte` cho phép bạn liên kết một đối tượng ngữ cảnh với `component` hiện tại bằng một khóa `(key)` cụ thể. Đối tượng ngữ cảnh này sau đó có thể được truy cập từ các `component` con của `component` hiện tại (bao gồm cả nội dung được chèn vào) bằng cách sử dụng hàm `getContext`.

```svelte
function setContext<T>(key: any, context: T): T;
```

- Mô tả:
 + `setContext` liên kết đối tượng ngữ cảnh `(context)` với khóa `(key)` và trả về đối tượng đó.
 + Ngữ cảnh này sẽ được cung cấp cho các `component` con thông qua hàm `getContext`.
 + Hàm `setContext` cần được gọi trong quá trình khởi tạo của `component`, tức là trong phần `<script>` của `component`.

```svelte
<script>
	import { setContext } from 'svelte';

	setContext('answer', 42);
</script>
```

- Trong ví dụ này, đối tượng ngữ cảnh 42 được liên kết với khóa `'answer'` và có thể được truy cập từ các `component` con bằng cách sử dụng `getContext`.

- Sử dụng `getContext` để truy cập ngữ cảnh:

```svelte
<script>
	import { getContext } from 'svelte';

	const answer = getContext('answer');
	console.log(answer); // 42
</script>
```

- Lưu ý:

 + Ngữ cảnh không tự động phản ứng với các thay đổi. Nếu bạn cần giá trị phản ứng trong ngữ cảnh, bạn có thể truyền một `store` vào ngữ cảnh. `Store` là phản ứng và sẽ tự động cập nhật các `component` khi giá trị của nó thay đổi.

### getContext

- Hàm `getContext` trong `Svelte` được sử dụng để truy xuất ngữ cảnh `(context)` đã được thiết lập bởi một `component` cha bằng hàm `setContext`. Nó lấy ngữ cảnh dựa trên khóa `(key)` được chỉ định.

```svelte
function getContext<T>(key: any): T;
```

- Mô tả:
 + `getContext` lấy ngữ cảnh liên kết với khóa cụ thể từ `component` cha gần nhất.
 + Hàm này cần được gọi trong quá trình khởi tạo của `component`, tức là trong phần `<script>` của `component`.
 + Nếu không có ngữ cảnh nào được thiết lập với khóa đó, `getContext` sẽ gây lỗi.

- Trong `component` cha:

```svelte
<script>
	import { setContext } from 'svelte';

	setContext('answer', 42);
</script>

<ChildComponent />
```

- Trong `component` con `(ChildComponent)`:

```svelte
<script>
	import { getContext } from 'svelte';

	const answer = getContext('answer');
	console.log(answer); // 42
</script>
```

- Trong ví dụ này, `component` cha thiết lập ngữ cảnh với khóa `'answer'` và giá trị 42.

- `Component` con sử dụng `getContext` để truy xuất giá trị của ngữ cảnh đó bằng cùng khóa `'answer'`.

- Lưu ý:

 + `getContext` chỉ có thể truy cập ngữ cảnh từ các `component` cha. Nếu không có ngữ cảnh được thiết lập với khóa đó, hoặc nếu gọi `getContext` trước khi `setContext` được thực hiện trong một `component` cha, bạn sẽ gặp lỗi.
 + Hàm `getContext` cần được gọi trong quá trình khởi tạo của `component`, không thể sử dụng ngoài khối `<script>`.

### hasContext

- Hàm `hasContext` trong `Svelte` được sử dụng để kiểm tra xem một khóa `(key)` cụ thể có ngữ cảnh `(context)` được thiết lập trong các `component` cha hay không. Nó trả về một giá trị `boolean` cho biết ngữ cảnh với khóa đó có tồn tại hay không.

```svelte
function hasContext(key: any): boolean;
```

- Mô tả:
 + `hasContext` kiểm tra sự tồn tại của ngữ cảnh với khóa cụ thể.
 + Hàm này trả về `true` nếu ngữ cảnh với khóa đó đã được thiết lập bởi một `component` cha, và `false` nếu không có ngữ cảnh nào được thiết lập với khóa đó.
 + `hasContext` cần được gọi trong quá trình khởi tạo của `component`, tức là trong phần `<script>` của `component`.

- Trong component cha:

```svelte
<script>
	import { setContext } from 'svelte';

	setContext('answer', 42);
</script>

<ChildComponent />
```

- Trong `component` con `(ChildComponent)`:

```svelte
<script>
	import { hasContext, getContext } from 'svelte';

	if (hasContext('answer')) {
		const answer = getContext('answer');
		console.log(answer); // 42
	} else {
		console.log('No context found for "answer"');
	}
</script>
```

- Trong ví dụ này, `component` cha thiết lập ngữ cảnh với khóa `'answer'`.
- `Component` con sử dụng `hasContext` để kiểm tra sự tồn tại của ngữ cảnh với khóa `'answer'`. Nếu ngữ cảnh tồn tại, nó sẽ sử dụng `getContext` để truy xuất giá trị. Nếu không có ngữ cảnh với khóa đó, nó sẽ thông báo rằng không có ngữ cảnh được tìm thấy.

- Lưu ý:

 + `hasContext` chỉ có thể kiểm tra ngữ cảnh từ các `component` cha. Nó không thể kiểm tra ngữ cảnh từ các `component` không liên quan hoặc từ các `component` cha không được thiết lập ngữ cảnh.
 + Hàm `hasContext` cần được gọi trong quá trình khởi tạo của `component` và không thể sử dụng ngoài khối `<script>`.

### getAllContexts

```svelte
function getAllContexts<
	T extends Map<any, any> = Map<any, any>
>(): T;
```

Hàm `getAllContexts` trong `Svelte` sẽ lấy toàn bộ bản đồ ngữ cảnh `(context map)` thuộc về `component` cha gần nhất. Hàm này phải được gọi trong quá trình khởi tạo của `component`. Nó hữu ích, ví dụ, khi bạn tạo một `component` bằng cách lập trình và muốn truyền ngữ cảnh hiện có cho nó.

```svelte
<script>
	import { getAllContexts } from 'svelte';

	// Lấy bản đồ ngữ cảnh từ component cha gần nhất
	const contexts = getAllContexts();
</script>
```
- Giải thích:

`getAllContexts` trả về một bản đồ chứa tất cả các ngữ cảnh (`Map object)` từ `component` cha gần nhất.
Bạn có thể sử dụng bản đồ này để truy cập hoặc thay đổi ngữ cảnh, giúp quản lý trạng thái hoặc dữ liệu dùng chung giữa các `component` lồng nhau dễ dàng hơn.
Trong lập trình `Svelte`, hàm này thường được sử dụng khi bạn cần đảm bảo rằng một `component` được tạo động có quyền truy cập vào cùng một ngữ cảnh như `component` cha của nó.

### createEventDispatcher

```svelte
function createEventDispatcher<
	EventMap extends Record<string, any> = any
>(): EventDispatcher<EventMap>;
```

- Hàm này tạo ra một bộ phân phối sự kiện `(event dispatcher)` có thể được sử dụng để phân phối `(dispatch)` các sự kiện của `component`. Các bộ phân phối sự kiện là các hàm có thể nhận hai đối số: name (tên sự kiện) và `detail` (chi tiết sự kiện).

- Các sự kiện của `component` được tạo ra với `createEventDispatcher` sẽ tạo ra một sự kiện `CustomEvent`. Những sự kiện này không có tính chất lan truyền `(bubble)`. Đối số `detail` tương ứng với thuộc tính `CustomEvent.detail` và có thể chứa bất kỳ loại dữ liệu nào.

```svelte
<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();
</script>

<button on:click={() => dispatch('notify', 'giá trị chi tiết')}>Gửi Sự Kiện</button>
```

- Các sự kiện được phân phối từ các `component` con có thể được lắng nghe trong `component` cha. Dữ liệu được cung cấp khi sự kiện được phân phối có sẵn trong thuộc tính detail của đối tượng sự kiện.

```svelte
<script>
	function callbackFunction(event) {
		console.log(`Notify đã được kích hoạt! Chi tiết: ${event.detail}`);
	}
</script>

<Child on:notify={callbackFunction} />
```

- Sự kiện có thể bị hủy `(cancelable)` bằng cách truyền một đối số thứ ba vào hàm `dispatch`. Hàm này sẽ trả về false nếu sự kiện bị hủy với `event.preventDefault()`, ngược lại nó sẽ trả về `true`.

```svelte
<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	function notify() {
		const shouldContinue = dispatch('notify', 'giá trị chi tiết', { cancelable: true });
		if (shouldContinue) {
			// không có listener nào gọi preventDefault
		} else {
			// một listener đã gọi preventDefault
		}
	}
</script>
```

- Bạn có thể định kiểu cho bộ phân phối sự kiện để xác định các sự kiện mà nó có thể nhận. Điều này sẽ làm cho mã của bạn an toàn hơn về mặt kiểu dữ liệu (`type-safe)` cả trong `component` (các lời gọi sai sẽ bị cảnh báo) và khi sử dụng `component` (các loại sự kiện bây giờ sẽ được thu hẹp).

### Types

#### ComponentConstructorOptions

```svelte
interface ComponentConstructorOptions<
	Props extends Record<string, any> = Record<string, any>
> { … }
```

- `ComponentConstructorOptions` là một giao diện `(interface)` định nghĩa các tùy chọn mà bạn có thể cung cấp khi tạo một `instance` của một `component` trong `Svelte`.

- Các thuộc tính:

- target: Element | Document | ShadowRoot
 + Mô tả: Phần tử DOM mà `component` sẽ được render vào. Đây là thuộc tính bắt buộc.

- anchor?: Element (tùy chọn)
 + Mô tả: Nếu được cung cấp, `component` sẽ được chèn vào trước phần tử này thay vì trở thành phần tử con cuối cùng của target.

- props?: Props (tùy chọn)
 + Mô tả: Đối tượng chứa các thuộc tính (props) sẽ được truyền vào `component`. Các thuộc tính này sẽ khởi tạo giá trị của các props của `component`.

- context?: Map<any, any> (tùy chọn)
 + Mô tả: Bản đồ (map) chứa các giá trị ngữ cảnh (context) được truyền từ các `component` cha.

- hydrate?: boolean (tùy chọn)
 + Mô tả: Nếu được đặt là true, Svelte sẽ cố gắng "hydrate" (kết hợp lại) với DOM hiện tại thay vì tạo ra DOM mới. Điều này thường được sử dụng trong các ứng dụng render phía server (server-side rendering).

- intro?: boolean (tùy chọn)
 + Mô tả: Nếu được đặt là true, các animation intro sẽ được kích hoạt khi `component` được render lần đầu.

- $$inline?: boolean (tùy chọn)
 + Mô tả: Một cờ đặc biệt, thường được sử dụng nội bộ bởi Svelte, để xác định liệu `component` có nên được render như một inline `component` hay không.

 #### ComponentEvents

 - `ComponentEvents` là một kiểu dữ liệu tiện lợi trong `Svelte` được sử dụng để lấy các sự kiện mà một `component` cụ thể mong đợi. Nó giúp bạn xác định và làm việc với các sự kiện mà `component` sẽ phát ra.

```svelte
<script lang="ts">
   import type { ComponentEvents } from 'svelte';
   import Component from './Component.svelte';

   function handleCloseEvent(event: ComponentEvents<Component>['close']) {
	  console.log(event.detail);
   }
</script>

<Component on:close={handleCloseEvent} />
```

- Cách hoạt động:

```svelte
type ComponentEvents<Component extends SvelteComponent_1> =
	Component extends SvelteComponent<any, infer Events>
		? Events
		: never;
```

- `ComponentEvents<Component>`: Đây là một kiểu dữ liệu nhận vào một `component` và trả về các sự kiện mà `component` đó có thể phát ra.

- `Component extends SvelteComponent<any, infer Events>`: Đây là điều kiện kiểm tra xem `Component` có phải là một `SvelteComponent` hay không. Nếu đúng, nó sẽ sử dụng `infer` để trích xuất các sự kiện của `component` vào biến `Events`.

- `? Events : never`: Nếu `Component` là một `SvelteComponent`, nó sẽ trả về `Events` (các sự kiện của `component`). Nếu không, nó sẽ trả về `never`, có nghĩa là kiểu dữ liệu không hợp lệ.

- Ứng dụng trong ví dụ:

- `ComponentEvents<Component>['close']`: Dòng này lấy kiểu của sự kiện `close` từ `component` `Component`. Nó giúp đảm bảo rằng hàm `handleCloseEvent` có kiểu chính xác cho đối số event.

- Điều này giúp mã `TypeScript` của bạn an toàn hơn và tránh các lỗi khi làm việc với các sự kiện của `component` trong `Svelte`.

#### ComponentProps

- `ComponentProps` là một kiểu dữ liệu tiện lợi trong `Svelte`, được sử dụng để lấy các thuộc tính `(props)` mà một `component` cụ thể mong đợi. Nó giúp bạn xác định và làm việc với các props của `component` một cách an toàn hơn trong `TypeScript`.

```svelte
<script lang="ts">
	import type { ComponentProps } from 'svelte';
	import Component from './Component.svelte';

	const props: ComponentProps<Component> = { foo: 'bar' }; // Báo lỗi nếu đây không phải là các props đúng
</script>
```

- Cách hoạt động:

```svelte
type ComponentProps<Component extends SvelteComponent_1> =
	Component extends SvelteComponent<infer Props>
		? Props
		: never;
```

- `ComponentProps<Component>`: Đây là một kiểu dữ liệu nhận vào một `component` và trả về các thuộc tính `(props)` mà `component` đó có thể nhận.

- `Component extends SvelteComponent<infer Props>`: Đây là điều kiện kiểm tra xem `Component` có phải là một `SvelteComponent` hay không. Nếu đúng, nó sẽ sử dụng `infer` để trích xuất các props của `component` vào biến `Props`.

- `? Props : never: Nếu Component là một SvelteComponent`, nó sẽ trả về `Props` (các `props` của `component`). Nếu không, nó sẽ trả về `never`, có nghĩa là kiểu dữ liệu không hợp lệ.

- Ứng dụng trong ví dụ:
- `ComponentProps<Component>`: Trong ví dụ này, dòng này xác định kiểu của các `props` mà `Component` mong đợi. Khi bạn gán giá trị cho `props`, `TypeScript` sẽ kiểm tra xem các giá trị đó có khớp với các props mà `Component` yêu cầu hay không. Nếu không khớp, `TypeScript` sẽ báo lỗi.
Điều này giúp bạn đảm bảo rằng bạn đang truyền đúng các `props` vào `component`, giúp mã của bạn an toàn hơn và tránh các lỗi tiềm ẩn khi sử dụng `TypeScript` trong `Svelte`.

#### ComponentType

- `ComponentType` là một kiểu dữ liệu tiện lợi trong `Svelte`, được sử dụng để xác định kiểu của một `component Svelte`. Kiểu này rất hữu ích khi bạn làm việc với các `component` động, ví dụ như khi sử dụng `<svelte:component>`.

```svelte
<script lang="ts">
	import type { ComponentType, SvelteComponent } from 'svelte';
	import Component1 từ './Component1.svelte';
	import Component2 từ './Component2.svelte';

	const component: ComponentType = someLogic() ? Component1 : Component2;
	const componentOfCertainSubType: ComponentType<SvelteComponent<{ needsThisProp: string }>> = someLogic() ? Component1 : Component2;
</script>

<svelte:component this={component} />
<svelte:component this={componentOfCertainSubType} needsThisProp="hello" />
```

- Cách hoạt động:

```svelte
type ComponentType<
	Component extends SvelteComponent = SvelteComponent
> = (new (
	options: ComponentConstructorOptions<
		Component extends SvelteComponent<infer Props>
			? Props
			: Record<string, any>
	>
) => Component) & {
	/** The custom element version of the component. Only present if compiled with the `customElement` compiler option */
	element?: typeof HTMLElement;
};
```

- `ComponentType<Component>`: Đây là một kiểu dữ liệu dùng để mô tả kiểu của một `component Svelte`. Bạn có thể chỉ định một kiểu `component` cụ thể bằng cách truyền vào một kiểu mở rộng từ `SvelteComponent`.

- `new (options: ComponentConstructorOptions<Props>) => Component`: Định nghĩa này chỉ ra rằng kiểu `ComponentType` là một hàm khởi tạo mới `(new)`, nhận vào `options` là một đối tượng có kiểu `ComponentConstructorOptions<Props>`. Props này sẽ được suy luận `(infer)` từ kiểu `Component` nếu `Component` mở rộng từ `SvelteComponent`.

- `element?: typeof HTMLElement`: Đây là một thuộc tính tùy chọn trong `ComponentType`. Nó chỉ tồn tại nếu `component` được biên dịch với tùy chọn `customElement` của trình biên dịch `(compiler)`.

- Ứng dụng trong ví dụ:
- `component: ComponentType`: Trong ví dụ này, `component` có kiểu là `ComponentType`, cho phép bạn chọn một trong hai `component` (`Component1` hoặc `Component2`) dựa trên `logic` nào đó `(someLogic())`.

- `componentOfCertainSubType: ComponentType<SvelteComponent<{ needsThisProp: string }>>`: Ở đây, `componentOfCertainSubType` có kiểu là một `ComponentType` cụ thể với `SvelteComponent` yêu cầu một `prop` là `needsThisProp` có kiểu `string`. Điều này đảm bảo rằng khi bạn sử dụng `component` đó, bạn phải truyền vào `prop needsThisProp` với giá trị hợp lệ.

Điều này giúp mã `TypeScript` của bạn mạnh mẽ và an toàn hơn khi làm việc với các `component` động và `props` trong `Svelte`.

#### SvelteComponent

- `SvelteComponent` là lớp cơ sở cho các `component` trong `Svelte` với một số cải tiến nhỏ cho việc phát triển `(dev-enhancements)`. Nó được sử dụng khi `dev=true`, tức là trong môi trường phát triển để cung cấp các công cụ hỗ trợ lập trình.

- Bạn có một thư viện `component` trên `npm` gọi là `component-library`, trong đó xuất ra một `component` gọi là `MyComponent`. Để cung cấp kiểu dữ liệu `(typings)` cho người dùng `Svelte+TypeScript`, bạn tạo một `file` `index.d.ts` như sau:

```svelte
import { SvelteComponent } from "svelte";

export class MyComponent extends SvelteComponent<{foo: string}> {}
```

- Điều này cho phép các `IDE` như `VS Code` với phần mở rộng `Svelte` cung cấp tính năng tự động hoàn thành `(intellisense)` và sử dụng `component` trong một `file Svelte` với `TypeScript` như sau:

```svelte
<script lang="ts">
	import { MyComponent } from "component-library";
</script>

<MyComponent foo={'bar'} />
```

- Cách hoạt động:

```svelte
class SvelteComponent<
	Props extends Record<string, any> = any,
	Events extends Record<string, any> = any,
	Slots extends Record<string, any> = any
> extends SvelteComponent_1<Props, Events> {
	[prop: string]: any;
	constructor(options: ComponentConstructorOptions<Props>);
	$capture_state(): void;
	$inject_state(): void;
}
```

- `Props extends Record<string, any> = any`: Đây là kiểu dữ liệu của các thuộc tính `(props)` mà `component` có thể nhận. Mặc định là `any`, nhưng bạn có thể chỉ định kiểu cụ thể cho `Props` khi kế thừa `SvelteComponent`.

- `Events extends Record<string, any> = any`: Đây là kiểu dữ liệu của các sự kiện mà `component` có thể phát ra. Mặc định là `any`.

- `Slots extends Record<string, any> = any`: Đây là kiểu dữ liệu của các slots mà `component` có thể chứa. Mặc định là `any`.

- `[prop: string]: any`: Điều này cho phép `SvelteComponent` nhận bất kỳ thuộc tính nào với kiểu any.

- `constructor(options: ComponentConstructorOptions<Props>)`: Hàm khởi tạo của `SvelteComponent` nhận vào các tùy chọn để cấu hình `component`.

- `$capture_state()`: Phương thức đặc biệt được sử dụng trong quá trình phát triển để chụp `(capture)` trạng thái của `component`.

- `$inject_state()`: Phương thức đặc biệt được sử dụng trong quá trình phát triển để chèn `(inject)` trạng thái vào `component`.

- `SvelteComponent` là lớp cơ sở giúp bạn tạo các `component` với kiểu dữ liệu rõ ràng và hỗ trợ công cụ phát triển. Việc định nghĩa kiểu cho các `props` và sự kiện giúp mã của bạn an toàn hơn và hỗ trợ tính năng tự động hoàn thành trong `IDE`.

Nếu bạn cần thêm thông tin hoặc có câu hỏi khác, hãy cho mình biết nhé!

#### SvelteComponentTyped

`SvelteComponentTyped` là một lớp cơ sở đã được sử dụng trước đây trong `Svelte` để cung cấp kiểu dữ liệu mạnh mẽ cho các `component`. Tuy nhiên, lớp này đã được thay thế bằng `SvelteComponent` trong các phiên bản mới hơn của `Svelte`. Để biết thêm thông tin chi tiết về sự thay đổi này, bạn có thể xem PR #8512 trên GitHub.

```svelte
class SvelteComponentTyped<
	Props extends Record<string, any> = any,
	Events extends Record<string, any> = any,
	Slots extends Record<string, any> = any
> extends SvelteComponent<Props, Events, Slots> {}
```

- `Props extends Record<string, any> = any`: Đây là kiểu dữ liệu cho các thuộc tính `(props)` mà `component` có thể nhận. Mặc định là `any`, nhưng bạn có thể chỉ định kiểu cụ thể cho `Props`.

- `Events extends Record<string, any> = any`: Đây là kiểu dữ liệu cho các sự kiện mà `component` có thể phát ra. Mặc định là `any`.

- `Slots extends Record<string, any> = any`: Đây là kiểu dữ liệu cho các slots mà `component` có thể chứa. Mặc định là `any`.

- `extends SvelteComponent<Props, Events, Slots>`: `SvelteComponentTyped` kế thừa từ `SvelteComponent`, và cung cấp các kiểu mạnh mẽ cho `Props`, `Events`, và `Slots`.

- Lịch sử và Hiện tại:

- Trước đây: `SvelteComponentTyped` đã được sử dụng để định nghĩa các `component` với các kiểu dữ liệu cho `props`, sự kiện, và `slots`.

- Hiện tại: `SvelteComponent` là lớp cơ sở được khuyến khích sử dụng thay cho `SvelteComponentTyped`. Nó cung cấp cùng tính năng với một số cải tiến và được hỗ trợ tốt hơn trong các phiên bản `Svelte` mới.

## svelte/store

Mô-đun `svelte/store` `export` các hàm để tạo các store `readable`, `writable`, và `derived`.

- Hãy nhớ rằng bạn không cần phải sử dụng những hàm này để tận hưởng cú pháp phản ứng `$store` trong các thành phần của bạn. Bất kỳ đối tượng nào thực hiện đúng các phương thức `.subscribe`, `.unsubscribe`, và (tùy chọn) `.set` đều là một `store` hợp lệ, và sẽ hoạt động cả với cú pháp đặc biệt và với các `store derived` tích hợp sẵn của `Svelte`.

Điều này cho phép bạn bao bọc gần như bất kỳ thư viện quản lý trạng thái phản ứng nào khác để sử dụng trong `Svelte`. Đọc thêm về hợp đồng `store` để xem cách một triển khai đúng cách trông như thế nào.

### writable

```svelte
function writable<T>(
	value?: T | undefined,
	start?: StartStopNotifier<T> | undefined
): Writable<T>;
```

- Hàm `writable<T>` là một hàm tạo `store` có thể thiết lập giá trị từ các thành phần 'bên ngoài'. `Store` được tạo ra dưới dạng một đối tượng với các phương thức bổ sung `set` và `update`.

- `set` là một phương thức nhận một đối số là giá trị cần được thiết lập. Giá trị của `store` sẽ được đặt thành giá trị của đối số nếu giá trị của `store` chưa bằng giá trị đó.

- `update` là một phương thức nhận một đối số là một hàm `callback`. Hàm `callback` nhận giá trị hiện tại của `store` làm đối số và trả về giá trị mới để đặt vào `store`.

```svelte
// store.ts
import { writable } from 'svelte/store';

const count = writable(0);

count.subscribe((value) => {
	console.log(value);
}); // in ra '0'

count.set(1); // in ra '1'

count.update((n) => n + 1); // in ra '2'
```

- Nếu một hàm được truyền vào làm đối số thứ hai, hàm đó sẽ được gọi khi số lượng người đăng ký chuyển từ không có người đăng ký nào sang có một người đăng ký (nhưng không từ một người đăng ký sang hai người đăng ký, v.v.). Hàm đó sẽ nhận một hàm `set` để thay đổi giá trị của `store`, và một hàm `update` hoạt động giống như phương thức `update` của `store`, nhận một hàm `callback` để tính toán giá trị mới của `store` từ giá trị cũ. Hàm đó phải trả về một hàm `stop` được gọi khi số lượng người đăng ký chuyển từ một người sang không có người đăng ký nào.

Ví dụ với hàm `callback`:

```svelte
// store.ts
import { writable } from 'svelte/store';

const count = writable(0, () => {
	console.log('got a subscriber');
	return () => console.log('no more subscribers');
});

count.set(1); // không làm gì

const unsubscribe = count.subscribe((value) => {
	console.log(value);
}); // in ra 'got a subscriber', sau đó là '1'

unsubscribe(); // in ra 'no more subscribers'
```

### readable

```svelte
function readable<T>(
	value?: T | undefined,
	start?: StartStopNotifier<T> | undefined
): Readable<T>;
```

- Hàm `readable<T>` tạo ra một store mà giá trị của nó không thể được thiết lập từ các thành phần 'bên ngoài'. Tham số đầu tiên là giá trị khởi tạo của `store`, và tham số thứ hai của hàm `readable` tương tự như tham số thứ hai của hàm `writable`.

```svelte
import { readable } from 'svelte/store';

// Tạo một store `time` với giá trị khởi tạo là ngày hiện tại
const time = readable(new Date(), (set) => {
	set(new Date());

	// Cập nhật giá trị của store mỗi giây
	const interval = setInterval(() => {
		set(new Date());
	}, 1000);

	// Làm sạch interval khi không còn người đăng ký nào
	return () => clearInterval(interval);
});

// Tạo một store `ticktock` với giá trị khởi tạo là 'tick'
const ticktock = readable('tick', (set, update) => {
	// Cập nhật giá trị của store mỗi giây
	const interval = setInterval(() => {
		update((sound) => (sound === 'tick' ? 'tock' : 'tick'));
	}, 1000);

	// Làm sạch interval khi không còn người đăng ký nào
	return () => clearInterval(interval);
});
```

- Giải thích:

- `readable` tạo một `store` có giá trị chỉ có thể được cập nhật từ bên trong hàm `readable`, không thể được thay đổi trực tiếp từ các thành phần bên ngoài.
- `set` là một hàm dùng để thiết lập giá trị của `store`.
- `update` là một hàm dùng để cập nhật giá trị của `store` bằng cách áp dụng một hàm `callback` lên giá trị hiện tại.
- Trong ví dụ đầu tiên, `store time `được cập nhật mỗi giây với ngày và giờ hiện tại.
- Trong ví dụ thứ hai, `store ticktock` chuyển đổi giá trị giữa `'tick'` và `'tock'` mỗi giây.

