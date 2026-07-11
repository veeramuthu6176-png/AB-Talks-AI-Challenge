<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Prior Authorization Workflow Simulator</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.">
  <style>
    /* COLOR PALETTE: Shades of blue, black text */
    :root {
      --blue-bg: #e9f2fb;
      --blue-lane: #b6d3f5;
      --blue-stage: #d7e9fa;
      --blue-accent: #2176ae;
      --blue-dark: #145388;
      --white: #fff;
      --black: #222;
      --gray: #8cab3;
    }
    body {
      margin: ;
      font-family: 'Segoe UI', Arial, sans-serif;
      background: var(--blue-bg);
      color: var(--black);
    }
    .container {
      max-width: 108px;
      margin: auto;
      padding: 8px;
    }
    h1 {
      text-align: center;
      margin-bottom: 6px;
      color: var(--blue-dark);
      font-size: 2rem;
      letter-spacing: .03em;
    }
    .progress-tracker {
      display: flex;
      align-items: center;
      justify-content: center;
      margin: 8px  14px ;
      gap: 4px;
      flex-wrap:
