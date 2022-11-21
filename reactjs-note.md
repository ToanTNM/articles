
# ReactJS note

## 1. Cài đặt

  Cài đặt NodeJS

  ```cmd
  npm install create-react-app
  npm create-react-app it4u
  cd it4u
  ```

  Cài đặt thư viện dùng trong project. Các thư viện được cài đặt sẽ xuất hiện trong file `package.json`

  ```cmd
  npm install antd --save
  ```

  Khi clone 1 project đã có, chạy lệnh sau để cài đặt các gói thư viện

  ```cmd
  npm i
  ```

  Chạy project

  ```cmd
  npm start
  ```

  Có thể cài đặt và sử dụng ```npx``` hoặc ```yarn``` thay cho npm

## 2. IT4U

### 2.1. Main

  Thư viện

  ```cmd
  npm i antd @craco/craco
  ```

### 2.2. PWA

  Tạo file ```manifest.json``` ở thư mục root của project. Chi tiết cấu hình file [manifest.json](https://developer.mozilla.org/en-US/docs/Web/Manifest)

  Trình duyệt Chrome bắt buộc cung cấp ít nhất 2 icon kích thước 192x192px và 512x512px

  Vd:

  ```json
  {
    "name": "IT4U",
    "short_name": "IT4U",
    "icons": [{
        "src": "/images/icons/icon-192x192.png",
        "sizes": "192x192",
        "type": "image/png"
      }, {
        "src": "/images/icons/icon-512x512.png",
        "sizes": "512x512",
        "type": "image/png"
      }],
    "start_url": "/index.html",
    "display": "standalone",
    "background_color": "#3E4EB8",
    "theme_color": "#2F3BA2"
  }
  ```

  Edit file ```public/index.html```

  ```html
  <link rel="manifest" href="/manifest.json">
  ```

  Trên cửa sổ ```DevTools``` của ```Chrome```, chọn tab ```Application``` để kiểm tra file manifest đã load thành công chưa

  Trình duyệt Safari trên iOS chưa hỗ trợ file manifest này thì có thể bổ sung một số tag sau

  ```html
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="black">
  <meta name="apple-mobile-web-app-title" content="IT4U PWA">
  <link rel="apple-touch-icon" href="/images/icons/icon-192x192.png">
  ```

  Nếu cửa số Audit PWA có thông báo “Does not set an address-bar theme color”, chọn màu thanh address cho phù hợp với màu thương hiệu

  ```html
  <meta name="theme-color" content="#2F3BA2" />
  ```

## 3. Update state using object spread

  ```js
  setState({
    {
      ...state,
      currentUser: { //deep object
        ...state.currentUser,
        language: key
      },
    }
  })
  ```
