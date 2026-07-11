<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Prior Authorization Story Simulator</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
  .bubble-left::before{content:'';position:absolute;left:-8px;top:10px;border:8px solid transparent;border-right-color:#dbeafe}
  .bubble-right::before{content:'';position:absolute;right:-8px;top:10px;border:8px solid transparent;border-left-color:#bfdbfe}
  .fade-in{animation:fadeIn 0.3s ease-in}
  @keyframes fadeIn{from{opacity:0;transform:translateY(10px)}to{opacity:1;transform:translateY(0)}}
</style>
</head>
<body class="bg-slate-50 text-slate-900 font-sans">
<div class="max-w
