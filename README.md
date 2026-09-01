<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<title>복사 도우미 · Misom.lab</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
<style>
  :root{
    --sky-50:#eef4f8;
    --sky-100:#dce9f2;
    --ink:#1e2a35;
    --ink-soft:#5b6b78;
    --teal:#2a8f86;
    --teal-dark:#1f6d66;
    --amber:#f2a154;
    --card:#ffffff;
    --line:#d7e3ea;
    --radius:20px;
  }
  *{box-sizing:border-box;}
  html,body{height:100%;}
  body{
    margin:0;
    font-family:-apple-system,BlinkMacSystemFont,"Apple SD Gothic Neo","Pretendard","Malgun Gothic",sans-serif;
    background:
      radial-gradient(circle at 15% 10%, var(--sky-100), transparent 55%),
      radial-gradient(circle at 85% 90%, var(--sky-100), transparent 50%),
      var(--sky-50);
    color:var(--ink);
    min-height:100%;
    display:flex;
    align-items:center;
    justify-content:center;
    padding:24px;
  }
  .wrap{
    width:100%;
    max-width:460px;
  }
  .brand{
    display:flex;
    align-items:center;
    gap:8px;
    justify-content:center;
    margin-bottom:18px;
    color:var(--ink-soft);
    font-size:13px;
    letter-spacing:.02em;
  }
  .brand svg{display:block;}
  .card{
    background:var(--card);
    border-radius:var(--radius);
    padding:32px 28px;
    box-shadow:0 1px 2px rgba(30,42,53,.04), 0 12px 32px -12px rgba(30,42,53,.18);
    border:1px solid var(--line);
  }

  /* ---------- 공통 ---------- */
  h1{
    font-size:20px;
    margin:0 0 6px;
    font-weight:700;
  }
  p.desc{
    margin:0 0 22px;
    color:var(--ink-soft);
    font-size:14px;
    line-height:1.5;
  }
  textarea{
    width:100%;
    min-height:120px;
    resize:vertical;
    border:1.5px solid var(--line);
    border-radius:14px;
    padding:14px;
    font-size:15px;
    font-family:inherit;
    line-height:1.5;
    color:var(--ink);
    background:#fbfdfe;
    transition:border-color .15s ease;
  }
  textarea:focus{
    outline:none;
    border-color:var(--teal);
    background:#fff;
  }
  .meta-row{
    display:flex;
    justify-content:space-between;
    align-items:center;
    margin-top:8px;
    font-size:12px;
    color:var(--ink-soft);
  }
  .meta-row.warn{color:#b5772f;}

  button{
    font-family:inherit;
    cursor:pointer;
    border:none;
  }
  .btn-main{
    width:100%;
    margin-top:18px;
    padding:16px;
    border-radius:14px;
    background:var(--teal);
    color:#fff;
    font-size:16px;
    font-weight:700;
    transition:transform .1s ease, background .15s ease;
  }
  .btn-main:active{transform:scale(.98);}
  .btn-main:hover{background:var(--teal-dark);}
  .btn-main:disabled{background:#b8c4cb;cursor:not-allowed;}

  .btn-ghost{
    width:100%;
    margin-top:10px;
    padding:12px;
    border-radius:14px;
    background:transparent;
    border:1.5px solid var(--line);
    color:var(--ink-soft);
    font-size:14px;
    font-weight:600;
  }
  .btn-ghost:hover{border-color:var(--teal);color:var(--teal-dark);}

  .hint{
    display:flex;
    gap:8px;
    align-items:flex-start;
    background:#fff6ea;
    border:1px solid #f2dcb8;
    color:#8a5a1f;
    border-radius:12px;
    padding:10px 12px;
    font-size:12.5px;
    line-height:1.5;
    margin-bottom:18px;
  }
  .hint svg{flex:none;margin-top:1px;}

  /* ---------- 교사 화면: QR 결과 ---------- */
  .qr-result{display:none;text-align:center;}
  .qr-result.show{display:block;}
  .qr-box{
    display:inline-flex;
    padding:16px;
    background:#fff;
    border-radius:16px;
    border:1.5px solid var(--line);
    margin:6px 0 16px;
  }
  .qr-box img{display:block;width:220px;height:220px;}
  .link-box{
    background:#fbfdfe;
    border:1px solid var(--line);
    border-radius:10px;
    padding:10px 12px;
    font-size:12.5px;
    color:var(--ink-soft);
    word-break:break-all;
    text-align:left;
    max-height:70px;
    overflow-y:auto;
  }

  /* ---------- 학생 화면 ---------- */
  .preview{
    background:#fbfdfe;
    border:1.5px solid var(--line);
    border-radius:14px;
    padding:16px;
    font-size:15px;
    line-height:1.6;
    color:var(--ink);
    white-space:pre-wrap;
    word-break:break-word;
    max-height:220px;
    overflow-y:auto;
  }
  .copy-btn{
    width:100%;
    margin-top:20px;
    padding:22px;
    border-radius:16px;
    background:var(--teal);
    color:#fff;
    font-size:19px;
    font-weight:800;
    display:flex;
    align-items:center;
    justify-content:center;
    gap:10px;
    transition:transform .12s ease, background .15s ease;
  }
  .copy-btn:active{transform:scale(.97);}
  .copy-btn.done{background:var(--amber);}
  .copy-btn svg{flex:none;}

  .empty{
    text-align:center;
    padding:24px 0;
    color:var(--ink-soft);
    font-size:14px;
  }

  [data-view]{display:none;}
  [data-view].active{display:block;}
</style>
</head>
<body>

<div class="wrap">
  <div class="brand">
    <svg width="18" height="18" viewBox="0 0 24 24" fill="none"><path d="M7 17.5C4.24 17.5 2 15.36 2 12.7c0-2.36 1.75-4.32 4.06-4.65C6.7 5.4 9.1 3.5 12 3.5c2.9 0 5.3 1.9 5.94 4.55C20.25 8.38 22 10.34 22 12.7c0 2.66-2.24 4.8-5 4.8H7Z" fill="#8fbfd9"/></svg>
    <span>복사 도우미 · Misom.lab</span>
  </div>

  <div class="card">

    <!-- 교사 화면 -->
    <div data-view="teacher">
      <h1>학생에게 전달할 글</h1>
      <p class="desc">여기에 글을 입력하면 QR코드가 만들어져요. 학생이 QR을 찍으면 버튼 하나로 복사할 수 있어요.</p>

      <div id="fileWarning" class="hint" style="display:none;">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none"><path d="M12 9v4M12 17h.01M10.29 3.86l-8.2 14.2A2 2 0 0 0 3.8 21h16.4a2 2 0 0 0 1.71-2.94l-8.2-14.2a2 2 0 0 0-3.42 0Z" stroke="#8a5a1f" stroke-width="1.7"/></svg>
        <span>지금은 내 컴퓨터에서만 열려 있어요. 이 파일을 깃허브 페이지(promise8052.github.io) 같은 곳에 올려야 다른 기기에서 QR로 접속할 수 있어요.</span>
      </div>

      <textarea id="inputText" placeholder="예) 오늘의 문장을 따라 써 봅시다.&#10;나는 학교에 갑니다."></textarea>
      <div class="meta-row" id="metaRow">
        <span id="charCount">0자</span>
        <span id="charTip"></span>
      </div>

      <button class="btn-main" id="makeBtn">QR 만들기</button>

      <div class="qr-result" id="qrResult">
        <div class="qr-box"><img id="qrImg" alt="QR 코드"></div>
        <div class="link-box" id="linkBox"></div>
        <button class="btn-ghost" id="copyLinkBtn">링크 복사하기</button>
        <button class="btn-ghost" id="resetBtn">새로 만들기</button>
      </div>
    </div>

    <!-- 학생 화면 -->
    <div data-view="student">
      <h1>이 글을 복사해요</h1>
      <p class="desc">아래 내용을 확인하고, 버튼을 누르면 복사돼요.</p>
      <div class="preview" id="studentText"></div>
      <button class="copy-btn" id="studentCopyBtn">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none"><rect x="9" y="9" width="12" height="12" rx="2" stroke="white" stroke-width="1.8"/><path d="M5 15H4a1 1 0 0 1-1-1V4a1 1 0 0 1 1-1h10a1 1 0 0 1 1 1v1" stroke="white" stroke-width="1.8"/></svg>
        <span>복사하기</span>
      </button>
    </div>

    <!-- 내용 없음 -->
    <div data-view="empty">
      <div class="empty">전달할 내용을 찾을 수 없어요.<br>선생님께 QR을 다시 받아 주세요.</div>
    </div>

  </div>
</div>

<script>
(function(){
  const params = new URLSearchParams(location.search);
  const encoded = params.get('t');

  function showView(name){
    document.querySelectorAll('[data-view]').forEach(el=>{
      el.classList.toggle('active', el.getAttribute('data-view')===name);
    });
  }

  // ---------- 학생 화면 ----------
  if (encoded !== null) {
    let text = '';
    try { text = decodeURIComponent(encoded); } catch(e) { text = ''; }

    if (!text) {
      showView('empty');
    } else {
      showView('student');
      document.getElementById('studentText').textContent = text;

      const btn = document.getElementById('studentCopyBtn');
      const label = btn.querySelector('span');

      btn.addEventListener('click', async () => {
        let ok = false;
        try {
          if (navigator.clipboard && window.isSecureContext) {
            await navigator.clipboard.writeText(text);
            ok = true;
          }
        } catch(e) { ok = false; }

        if (!ok) {
          // 대체 복사 방식 (구형 브라우저 / 비보안 환경)
          const ta = document.createElement('textarea');
          ta.value = text;
          ta.style.position = 'fixed';
          ta.style.opacity = '0';
          document.body.appendChild(ta);
          ta.focus();
          ta.select();
          try { ok = document.execCommand('copy'); } catch(e) { ok = false; }
          document.body.removeChild(ta);
        }

        if (ok) {
          btn.classList.add('done');
          label.textContent = '복사했어요!';
          setTimeout(()=>{
            btn.classList.remove('done');
            label.textContent = '복사하기';
          }, 1800);
        } else {
          label.textContent = '복사에 실패했어요';
          setTimeout(()=>{ label.textContent = '복사하기'; }, 1800);
        }
      });
    }
    return;
  }

  // ---------- 교사 화면 ----------
  showView('teacher');

  if (location.protocol === 'file:') {
    document.getElementById('fileWarning').style.display = 'flex';
  }

  const inputText = document.getElementById('inputText');
  const charCount = document.getElementById('charCount');
  const charTip = document.getElementById('charTip');
  const metaRow = document.getElementById('metaRow');
  const makeBtn = document.getElementById('makeBtn');
  const qrResult = document.getElementById('qrResult');
  const linkBox = document.getElementById('linkBox');
  const qrImg = document.getElementById('qrImg');
  const copyLinkBtn = document.getElementById('copyLinkBtn');
  const resetBtn = document.getElementById('resetBtn');

  const WARN_LEN = 220;

  inputText.addEventListener('input', () => {
    const len = inputText.value.length;
    charCount.textContent = len + '자';
    if (len > WARN_LEN) {
      metaRow.classList.add('warn');
      charTip.textContent = '너무 길면 QR이 촘촘해져서 스캔이 어려울 수 있어요';
    } else {
      metaRow.classList.remove('warn');
      charTip.textContent = '';
    }
  });

  let currentUrl = '';

  makeBtn.addEventListener('click', () => {
    const text = inputText.value.trim();
    if (!text) {
      inputText.focus();
      return;
    }

    const base = location.origin + location.pathname;
    currentUrl = base + '?t=' + encodeURIComponent(text);

    document.getElementById('qrImg').parentElement.innerHTML = '';
    new QRCode(document.getElementById('qrImg').parentElement, {
      text: currentUrl,
      width: 220,
      height: 220,
      correctLevel: QRCode.CorrectLevel.M
    });

    linkBox.textContent = currentUrl;
    qrResult.classList.add('show');
    qrResult.scrollIntoView({behavior:'smooth', block:'nearest'});
  });

  copyLinkBtn.addEventListener('click', async () => {
    try {
      await navigator.clipboard.writeText(currentUrl);
      copyLinkBtn.textContent = '복사됐어요';
      setTimeout(()=>{ copyLinkBtn.textContent = '링크 복사하기'; }, 1500);
    } catch(e) {
      copyLinkBtn.textContent = '복사 실패';
      setTimeout(()=>{ copyLinkBtn.textContent = '링크 복사하기'; }, 1500);
    }
  });

  resetBtn.addEventListener('click', () => {
    inputText.value = '';
    charCount.textContent = '0자';
    charTip.textContent = '';
    metaRow.classList.remove('warn');
    qrResult.classList.remove('show');
    inputText.focus();
  });

})();
</script>
</body>
</html>
