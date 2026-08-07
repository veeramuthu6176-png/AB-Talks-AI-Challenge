const debugBtn = document.getElementById('debugBtn');
const loader = document.getElementById('loader');
const results = document.getElementById('results');
const errorBox = document.getElementById('errorBox');

debugBtn.addEventListener('click', async () => {
  const code = document.getElementById('code').value.trim();
  const error = document.getElementById('error').value.trim();
  const language = document.getElementById('language').value;
  const eli5 = document.getElementById('eli5').checked;

  errorBox.style.display = 'none';
  results.style.display = 'none';

  if (!code ||!error) {
    showError('Please paste both code and error message');
    return;
  }

  debugBtn.disabled = true;
  debugBtn.innerText = 'Debugging...';
  loader.style.display = 'block';

  try {
    const response = await fetch('http://127.0.0.1:5000/debug', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ code, error, language, eli5 })
    });

    const data = await response.json();

    if (!response.ok) {
      throw new Error(data.error || 'Server error');
    }

    document.getElementById('what').innerText = data.what;
    document.getElementById('lineInfo').innerText = `Error on Line(s): ${data.line_numbers.join(', ')}`;
    document.getElementById('why').innerText = data.why;
    document.getElementById('fix').innerText = data.fixed_code;
    results.style.display = 'block';

  } catch (err) {
    showError('Debug Failed: ' + err.message);
  } finally {
    loader.style.display = 'none';
    debugBtn.disabled = false;
    debugBtn.innerText = 'Debug This →';
  }
});

function showError(message) {
  errorBox.innerText = message;
  errorBox.style.display = 'block';
}

document.getElementById('copyBtn').addEventListener('click', () => {
  const fixCode = document.getElement
