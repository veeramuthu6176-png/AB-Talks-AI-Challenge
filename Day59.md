const debugBtn = document.getElementById('debugBtn');
const loader = document.getElementById('loader');
const results = document.getElementById('results');
const errorBox = document.getElementById('errorBox');
const fixElement = document.getElementById('fix');
const languageSelect = document.getElementById('language');
const historyList = document.getElementById('historyList');

const API_URL = 'https://debuglens-api.onrender.com/debug';
const HISTORY_KEY = 'debuglens_history';
let lastClickTime = 0;

window.addEventListener('load', () => {
  loadHistory();
  loadFromURL();
});

debugBtn.addEventListener('click', async () => {
  // Rate limit: 3 seconds
  const now = Date.now();
  if (now - lastClickTime < 3000) {
    showError('Please wait 3 seconds before debugging again');
    return;
  }
  lastClickTime = now;

  const code = document.getElementById('code').value.trim();
  const error = document.getElementById('error').value.trim();
  const language = languageSelect.value;
  const eli5 = document.getElementById('eli5').checked;

  // FORM VALIDATION
  errorBox.style.display = 'none';
  results.style.display = 'none';
  if (!code || code.length < 5) {
    showError('Please paste
