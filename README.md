An ơi, chạy đâu? — gói đóng gói app Android
Thư mục này chứa game (HTML5), icon và cấu hình Capacitor để tạo file APK/AAB. Có hai cách, chọn một.
Cách 0 (không cần cài gì): để GitHub tự build APK
Tạo một kho (repository) mới trên github.com, rồi tải toàn bộ nội dung đã giải nén của gói này lên (nhớ cả thư mục ẩn `.github`).
Vào tab Actions, chọn "Build APK", bấm Run workflow (hoặc cứ đẩy code lên là nó tự chạy). Chờ khoảng 5 đến 10 phút.
Mở lần chạy vừa xong, kéo xuống mục Artifacts, tải `an-oi-chay-apk` về, giải nén ra `app-debug.apk`.
Chép file vào điện thoại Android, cho phép cài từ nguồn không rõ và cài như bình thường.
Đây là bản debug, dùng để cài thử và chơi được ngay. Muốn đăng lên Google Play thì làm theo Cách 2 để ký app bằng keystore của bạn.
Cách 1 (nhanh): PWABuilder, không cần cài Android Studio
Đưa thư mục `www/` lên một địa chỉ https (GitHub Pages, Netlify, Vercel, Cloudflare Pages đều miễn phí).
Vào https://www.pwabuilder.com, dán địa chỉ vừa có, bấm Start.
Chọn Package for stores, Android, tải về gói APK/AAB. Icon và chế độ ngang lấy từ `manifest.webmanifest`.
Cách 2: Capacitor + Android Studio (chủ động hơn)
Cần cài Node.js 18+ và Android Studio.
```
npm install
npx cap add android
npm run icons        # tạo icon + màn hình chờ từ thư mục assets/
npx cap sync android
npx cap open android
```
Trong Android Studio: Build > Generate Signed Bundle / APK để ra file cài đặt (APK để cài thử, AAB để đưa lên Google Play).
Khoá chơi ngang màn hình
Mở `android/app/src/main/AndroidManifest.xml`, thêm vào thẻ `<activity ...>`:
```
android:screenOrientation="sensorLandscape"
```
Ẩn thanh trạng thái (toàn màn hình, tuỳ chọn)
Trong `android/app/src/main/java/.../MainActivity.java`:
```java
import android.os.Bundle;
import androidx.core.view.WindowCompat;
import androidx.core.view.WindowInsetsCompat;
import androidx.core.view.WindowInsetsControllerCompat;
import com.getcapacitor.BridgeActivity;

public class MainActivity extends BridgeActivity {
  @Override
  public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    WindowInsetsControllerCompat c = WindowCompat.getInsetsController(getWindow(), getWindow().getDecorView());
    c.hide(WindowInsetsCompat.Type.systemBars());
    c.setSystemBarsBehavior(WindowInsetsControllerCompat.BEHAVIOR\_SHOW\_TRANSIENT\_BARS\_BY\_SWIPE);
  }
}
```
Cần lưu ý
Tên gói (appId): đang là `vn.anoichay.game`. Đổi trong `capacitor.config.json` trước khi chạy `npx cap add android` nếu bạn có tên riêng. Tên này không đổi được sau khi đã đăng lên Google Play.
Font chạy offline: game dùng font VT323. Tải file `VT323-Regular.ttf` từ Google Fonts, đặt vào `www/fonts/`. Nếu bỏ qua, game vẫn chạy nhưng dùng font mặc định khi không có mạng.
Icon: `resources/icon-only.png` (icon đầy đủ), `icon-foreground.png` và `icon-background.png` (icon thích ứng của Android), `splash.png` (màn hình chờ). Trong `www/icons/` là các cỡ cho PWA.
Điểm kỷ lục và xu lưu bằng localStorage trong app, gỡ app là mất.
Đưa lên Google Play: cần tài khoản nhà phát triển, ký app bằng keystore của bạn (giữ kỹ file này) và trang chính sách quyền riêng tư. Game không thu thập dữ liệu nào.
