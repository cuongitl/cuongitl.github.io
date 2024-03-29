SafeGuard
===============

✨ SafeGuard Crypto Bot! Hỗ trợ:  BitGet, Binance


BOT tự động cài đặt Stop-Loss, cài đặt Take-Profit, dời stop-loss dương Break-Event (BE+), Trailing-Stop theo % cài đặt sẵn, hoặc DCA lệnh theo %(nếu cho phép).


**🦅 Safety first, then profit will come.🍀**


`TÍNH NĂNG`
-------------------
💥 **Bot gồm có các thông số sau:**


 ``1. equity_protect(%):`` khi tổng toàn bộ lệnh bị âm vượt thông số này thì bot sẽ đóng hết lệnh.


 ``2. Break-Event(BE):`` khi lệnh dương, bot tự động dời stoploss về entry+% (BE+)
 
   Thông số sử dụng:
   
   - be_trigger(%): mặc định=0.9
   - be_protect(%): mặc định=0.4

   với thông số trên, khi lệnh dương >= 0.9% thì bot sẽ dời sl về mức entry+0.4%
   
   BE+ hiểu nôm na là dời stop-loss dương.

   Ví dụ: Bạn vào lệnh BTC ở giá 10000, khi giá tăng lên 10090, tức là lệnh dương 0.9% thì bot sẽ:
   
   * Hủy stop-loss âm (nếu có)
   * Đặt stop-loss dương ở mức bảo vệ 0.4% ở giá 10040


 ``3. Trailing-Stop(TS):``
 
   Bot sẽ tự động điều chỉnh stoploss(dương) liên tục để bám sát biến động giá thị trường theo hướng có lợi cho nhà đầu tư giúp gia tăng lợi nhuận tối đa. Vì đây là    bot bảo vệ, nên TS hoạt động theo cơ chế bảo vệ lệnh - chỉ dời stoploss dương - KHÔNG MUA/BÁN LỆNH MỚI.
 
   TS chỉ hoạt động sau khi lệnh đã có BE+, thông số sử dụng:
   
   - ts_rate(%):  Mặc định = 0 - KHÔNG CHO PHÉP TS.
   
   Trailing Stop được xem là lệnh cắt lỗ động (dynamic stop loss), nó di chuyển cùng chiều với xu hướng lệnh và **giữ một khoảng cách xác định trước so với giá thị trường**. Khoảng cách được cài đặt ở đây = ``ts_rate(%)``. 
   
   Lưu ý: giá thay đổi tối thiểu ``0.1%`` thì bot mới chạy TS.

   Ví dụ: tiếp theo về lệnh BE ở trên: khi lệnh đã có stop-loss dương ở giá 10040, và khi đó giá thị trường tiếp tục tăng lên 11000.
   Lúc này, theo cơ chế TS thì bot sẽ luôn giữ khoảng cách bảo vệ ở mức 0.4%: 
   Cách tính: 11000 trừ lại 0.4% giá, tức là bot sẽ cài sl dương ở mức 10956

 ``4. Stop-Loss(%):`` Chia làm 02 cơ chế stop-loss
   
   ``4.1 Hidden stop-loss``: bot sẽ không đặt sẵn stop-loss. Bot sẽ theo dõi và tự động cắt lệnh (sl) theo %giá hoặc theo %vốn vào lệnh. Phương pháp
   này gọi là stop-loss động nhằm tránh chiến lược Stop-Hunt của các cá mập.
   
   - sl_by: percent hoặc margin, mặc định=margin. Thông số này quyết định bot sẽ tính toán sl theo % giá cố định hoặc theo % của vốn lệnh(margin) hiện tại.
   - sl_percent: mức % stop-loss. Mặc định = 0  - KHÔNG SỬ DỤNG STOP-LOSS.

    Ví dụ 1: sl_by = margin, sl_percent = 25%. Bạn vào lệnh 12$, khi lệnh bị âm -3$ (~ -25%) thì bot sẽ cắt lệnh này.
    
    Ví dụ 2: sl_by = percent, sl_percent = 25%. Bạn mua ETH ở giá 1000 thì bot sẽ đặt stop-loss ở mức -25% giá, tức là stop-loss ở giá 750.
    
   ``4.2 Hard stop-loss``: bot sẽ đặt sẵn stop-loss cứng mặc định ở mức -35% giá.
   
    Ví dụ: Bạn mua BTC ở giá 20000 thì bot sẽ đặt stop-loss ở mức -35% giá, tức là stop-loss ở giá 13000.

    Hard stop-loss(sl cứng) là mặc định bot đặt, bạn cần để thông số này lớn hơn thông số của sl động ở trên.
    
    Ví dụ bạn muốn sl 25% nhưng giá biến động quá nhanh, nó chưa kích hoạt dc lệnh cắt lỗ ở mức 25% đó thì có sẵn sl cứng đặt ở -35%

 ``5. Take-Profit(%):``
 
    - tp_percent: Mặc định = 10%

     Bot tự đặt tp ở mức entry+10%  với thông số trên.


 ``6. dca_percent(%):`` Mặc định = 0 - KHÔNG CHO PHÉP DCA.
 
    - max_margin($): khống chế vốn tối đa của 1 lệnh (chỉ dùng khi bật tính năng DCA). Mặc định = 50$.
    - multi: mặc định=1.5. Thông số này quyết định DCA có gấp thếp vốn không?
    - max_dca_per_day: mặc định=2. Số lần DCA tối đa trong 1 ngày.
    - minutes_between_dca: mặc định=59. Thời gian tối thiểu(theo phút) giữa 2 lần DCA.

    Khi margin của 1 lệnh(vị thế) LỚN HƠN HOẶC BẰNG max_margin thì bot sẽ không nhồi lệnh (DCA) cho vị thế lệnh đó nữa.
    
    Ví dụ: Bạn cài bot với max_margin=50$, multi=1.5. Bạn vào lệnh vốn 22$, khi lệnh bị âm thì bot sẽ nhồi với vốn 22 x1.5 = 33$ ==>
    tổng vốn của lệnh sẽ là 55$. Nhưng mức khống chế vốn ở mức 50$ nên bot không thể nhồi lệnh!


 ``7. symbols_skip:`` bot sẽ bỏ qua các coin trong danh sách này.



* ``Lưu ý``: khi lệnh dương thì bot sẽ kiểm tra Take-Profit và Break-Event, khi lệnh âm thì bot sẽ kiểm tra Stop-Loss và DCA (nếu cho phép).


Bên trên là các thông số cơ bản, ngoài ra còn một số thông số khác nhằm hạn chế rủi ro, không cho phép chỉnh.



❇️ ACE ai chưa có tài khoản giao dịch thì đăng ký link ref(*) ủng hộ mình nhé:

 * Sàn Bitget: https://signal.lecuong.info/s/bg
 
 * Sàn Binance:  https://signal.lecuong.info/s/bnb



`XEM`
-------------------

Để xem thông số bảo vệ của tài khoản:

.. code-block:: console
   /guard <tên-tài-khoản>

Ví dụ xem thông số bảo vệ của tài khoản tên là bitget_m1:
 
 .. code-block:: console

   /guard bitget_m1


`THAY ĐỔI`
-------------------

Để thay đổi thông số, có 02 cách:

* Sử dụng bot Telegram: `@Cuongitl_bot <https://t.me/Cuongitl_bot>`_.

 với cú pháp như sau:

 .. code-block:: console
 
   /guard <tên-thông-số> <giá-trị-mới>

* Sử dụng web: `TradingSignals - Your way to success! <https://signal.lecuong.info/svc>`_.

Sau khi đăng ký và đăng nhập vào web, ở góc trên (bên phải) chỗ tên tk --> View Data

* - Chọn *SafeGuard Params*
* - Click vào *Get Data*, web sẽ hiển thị tất cả tk sàn sử dụng bot.
* - Click vô chữ *Update* (cột cuối cùng) của tk muốn sửa thông số.


`VÍ DỤ`
---------------------

Thao tác các lệnh sau với bot Telegram.


Ví dụ #1: khống chế vốn nhồi lệnh tối đa khi DCA ở mức 50$
 
 .. code-block:: console

   /guard max_margin 50
 
Ví dụ #2: Thay đổi phương thức sl là margin, %sl = 25%
 
 .. code-block:: console

   /guard sl_margin 25

Ví dụ #3: Thay đổi phương thức sl là price, %sl = 2%
 
 .. code-block:: console

   /guard sl_price 2

Ví dụ #4: Thay đổi stop-loss cứng ở mức 20%
 
 .. code-block:: console

   /guard hard_sl 20
   
Ví dụ #5: Thay đổi break-event về tỷ lệ: trigger(bẫy) = 1%, bảo vệ ở mức: 0.5%
 
 .. code-block:: console

   /guard be 1 0.5


Ví dụ #6: BẬT chế độ Trailing-Stop ở mức 0.4%
 
 .. code-block:: console

   /guard ts 0.4
   
   
Ví dụ #7: TẮT chế độ Trailing-Stop
 
 .. code-block:: console

   /guard ts 0
   
   
Ví dụ #8: Thêm coin LUNAUSDT vào danh sách loại trừ (không cần bot bảo vệ)
 
 .. code-block:: console

   /guard add LUNAUSDT


Ví dụ #9: Gỡ coin LUNAUSDT khỏi danh sách loại trừ.
 
 .. code-block:: console

   /guard remove LUNAUSDT

Ví dụ #10: KHÔNG SỬ DỤNG danh sách loại trừ.
 
 .. code-block:: console

   /guard remove all



`CÁC THÔNG SỐ`
---------------------


Danh sách thông số(*):

* equity_protect
* be
* ts
* max_margin
* sl_price
* sl_margin
* tp_percent
* dca_percent
* multi
* max_dca_per_day
* minutes_between_dca
* add
* remove
* help

 (*) Bạn cần phải gõ đúng tên thì bot Telegram mới thực thi lệnh.


Các thông số bảo vệ được lưu trữ trên hệ thống, bạn muốn thay đổi thì hãy chat với bot 
Telegram hoặc sử dụng web.

**🍀 Chúc mọi người luôn trade có lãi.**


* Tính toán phần trăm(%): `Percentage Calculator <https://signal.lecuong.info/ts/calc>`_.


