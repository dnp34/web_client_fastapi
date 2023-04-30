### web_client_fastapi
**Web-Client for FastAPI Server with Image Processing**


### Структура файлов для простого Web-приложения

**Чтобы сделать сайт доступным на другом порту (ex. 8080), нужно изменить конфигурацию веб-сервера (nginx.conf)**

<code>
Web-site-in-Docker/
│
├── index.html
│
├── css/
│   └── styles.css
│
├── js/
│   └── scripts.js
│
├── Dockerfile
│
├── docker-compose.yml
│
└── nginx.conf
</code>

---

### Examples

<p>
<img src="https://raw.githubusercontent.com/dnp34/web_client_fastapi/main/files/web1.jpg" width="58%">
<img src="https://raw.githubusercontent.com/dnp34/web_client_fastapi/main/files/web2.jpg" width="40%">
</p><br>

---

### Функция dataURLtoFile

Функция dataURLtoFile преобразует Data URL (base64-кодированное представление файла) в объект File, который можно использовать для передачи файла с помощью FormData на сервер. Вот что делает каждая строка функции:

1. let arr = dataurl.split(','):
- Разделяет Data URL на две части: заголовок и само закодированное содержимое. Заголовок включает тип MIME и схему кодирования.

2. mime = arr[0].match(/:(.*?);/)[1],
- Извлекает тип MIME из заголовка Data URL с использованием регулярного выражения.

3. bstr = atob(arr[1]),
- Декодирует base64-закодированное содержимое в строку бинарных данных.

4. n = bstr.length,
-Определяет длину декодированной строки бинарных данных.

5. u8arr = new Uint8Array(n);
- Создает пустой типизированный массив Uint8Array для хранения бинарных данных файла. Uint8Array используется для представления массива беззнаковых 8-битных целых чисел.

6. for (let i = 0; i < n; i++) {
- Цикл, который итерируется по каждому символу в строке бинарных данных.

7. u8arr[i] = bstr.charCodeAt(i);
- Конвертирует символ строки бинарных данных в его числовое значение (код символа) и сохраняет его в соответствующем индексе типизированного массива.

8. return new File([u8arr], filename, { type: mime });
- Создает объект File из типизированного массива, имени файла и типа MIME. Этот объект File возвращается функцией и может быть использован для передачи файла на сервер.

### Функция async function predict()

Функция async function predict() выполняет следующие действия:

1. Проверяет, выбрано ли изображение (if (!originalImage.src)) и возвращает ошибку, если изображение не выбрано.
2. Создает объект FormData и добавляет выбранное изображение в формате файла, преобразованное из Data URL, в него с именем "image".

3. Выполняет асинхронный POST-запрос к серверу по указанному URL (http://51.250.26.141:8001/process_image) с объектом FormData в качестве тела запроса. Асинхронность обеспечивается ключевым словом await, которое позволяет ждать завершения запроса, не блокируя выполнение другого кода.
4. Проверяет, является ли ответ успешным (код ответа 200-299). Если ответ не является успешным, генерируется ошибка с кодом ответа.

5. Получает сегментированное изображение в виде Blob из ответа сервера.
6. Создает URL-объект из полученного Blob и устанавливает его в качестве src для элемента segmentedImage, а также в качестве href для элемента downloadLink. Это позволяет отображать сегментированное изображение на странице и предоставлять ссылку для его скачивания.

В целом, функция predict() отвечает за отправку выбранного изображения на сервер, где происходит сегментация, и последующее отображение сегментированного изображения на веб-странице.
