---
title: "連絡フォーム"
comments: false
---

何か言伝がある場合はこちらからどうぞ。

<script src="https://www.google.com/recaptcha/api.js" async defer></script>
<p style="margin-top:3em;">
<form action="https://submit-form.com/74JqHCBZ" method="POST"  target="_blank">
  <p>
  <label for="name" style="font-size:small;">お名前</label>
  <input type="text" id="name" name="name" placeholder="" required="" style="background-color:#f0f0f0; width:100%" />
  </p>
  <p>
  <label for="email" style="font-size:small;">メール</label>
  <input type="email" id="email" name="email" placeholder="example@example.jp" required="" style="background-color:#f0f0f0; width:100%;" />
  </p>
  <p>
  <label for="message" style="font-size:small">メッセージ</label>
  <textarea
    id="message"
    name="message"
    placeholder="何でもお書き下さい。"
    required="true"
    style="background-color:#f0f0f0; width:100%; height:8em;"
  ></textarea></p>
  <p>
  <div
    class="g-recaptcha"
    data-sitekey="6Lcqne0UAAAAAIDQsmv0mX7PlXGJpImoBuIYPm64"
    data-callback="callback"
  ></div></p>
  <button type="submit" id="submit-button" style="padding:5px 20px 5px 20px; color:white; background-color:gray;" disabled>送信</button>
</form>
</p>

<script type="text/javascript">
      function callback() {
        const submitButton = document.getElementById("submit-button");
        submitButton.removeAttribute("disabled");
      }
</script>
