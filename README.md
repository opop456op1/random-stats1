<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>สุ่มสายอาชีพและค่าสถานะ</title>
  <style>
    body {
      font-family: sans-serif;
      text-align: center;
      margin: 20px;
    }
    #result {
      margin-top: 20px;
      font-weight: bold;
    }
    .result-box {
      margin-top: 10px;
      padding: 10px;
      border: 1px solid #000;
      display: inline-block;
      width: 100%;
      box-sizing: border-box;
      word-wrap: break-word;
    }
  </style>
</head>
<body>

<h1>สุ่มสายอาชีพและค่าสถานะ</h1>

<button onclick="generate()">สุ่ม!</button>

<div id="result"></div>

<script>
  const jobs = {
    "สายต่อสู้": [
      { name: "นักดาบ", rank: "A" },
      { name: "นักธนู", rank: "B" },
      { name: "นักหอก", rank: "A" },
      { name: "นักดาบคู่", rank: "A" },
      { name: "นักฆ่า", rank: "A" },
      { name: "นักรบมังกร", rank: "SS" },
      { name: "นักรบเงา", rank: "SS" },
      { name: "จอมดาบศักดิ์สิทธิ์", rank: "SSS" },
      { name: "จักรพรรดิดาบ", rank: "SSS" },
      { name: "นักสู้", rank: "B" },
      { name: "นักรบโล่", rank: "A" },
      { name: "จอมขุนพล", rank: "SSS" },
      { name: "อัศวินแห่งแสง", rank: "SS" },
      { name: "นักรบแห่งความมืด", rank: "SS" },
      { name: "นักเวทย์สงคราม", rank: "S" },
      { name: "ซามูไร", rank: "A" },
      { name: "นักรบทะเลทราย", rank: "B" }
    ],
    "สายเวทมนตร์": [
      { name: "จอมเวทย์", rank: "A" },
      { name: "จอมเวทย์ดำ", rank: "SS" },
      { name: "จอมเวทย์แสง", rank: "SS" },
      { name: "นักเวทย์น้ำแข็ง", rank: "A" },
      { name: "นักเวทย์ไฟ", rank: "A" },
      { name: "จอมเวทย์ดิน", rank: "SS" },
      { name: "นักเวทย์น้ำ", rank: "A" },
      { name: "นักเวทย์ลม", rank: "A" },
      { name: "จอมเวทย์สายฟ้า", rank: "SS" },
      { name: "หมอผี", rank: "B" },
      { name: "นักอัญเชิญ", rank: "A" },
      { name: "นักปราชญ์", rank: "A" },
      { name: "นักเวทย์พิษ", rank: "A" },
      { name: "นักเวทย์โลหะ", rank: "A" },
      { name: "นักเวทย์พลังจิต", rank: "SSS" }
    ],
    "สายสนับสนุน": [
      { name: "นักบวช", rank: "A" },
      { name: "นักบวชศักดิ์สิทธิ์", rank: "A" },
      { name: "ผู้รักษา", rank: "SS" },
      { name: "นักปรุงยา", rank: "B" },
      { name: "นักขับกล่อม", rank: "B" },
      { name: "นักเวทย์สนับสนุน", rank: "A" },
      { name: "นักซ่อมแซม", rank: "B" },
      { name: "ผู้ชำระล้าง", rank: "A" },
      { name: "นักเร้นลับ", rank: "A" },
      { name: "ผู้พิทักษ์จิตวิญญาณ", rank: "SS" },
      { name: "นักวางกลยุทธ์", rank: "A" }
    ],
    "สายเชี่ยวชาญ": [
      { name: "นักล่า", rank: "B" },
      { name: "นักล่าปีศาจ", rank: "A" },
      { name: "มือสังหาร", rank: "SS" },
      { name: "นักวางกับดัก", rank: "A" },
      { name: "นักสำรวจ", rank: "B" },
      { name: "นักประดิษฐ์", rank: "B" },
      { name: "นักวิจัยเวทมนตร์", rank: "A" },
      { name: "นักอัญมณี", rank: "B" },
      { name: "นักประดิษฐ์อาวุธ", rank: "A" },
      { name: "นักดูแลสัตว์", rank: "B" }
    ],
    "สายกึ่งอาชีพ": [
      { name: "ช่างตีเหล็ก", rank: "F" },
      { name: "ช่างไม้", rank: "F" },
      { name: "พ่อครัว", rank: "F" },
      { name: "ชาวนา", rank: "F" },
      { name: "พ่อค้า", rank: "E" },
      { name: "นักสะสม", rank: "F" },
      { name: "นักเขียน", rank: "F" },
      { name: "นักดนตรี", rank: "F" },
      { name: "นักล่าสมบัติ", rank: "D" },
      { name: "ช่างทำชุดเกราะ", rank: "E" }
    ],
    "สายพิเศษ": [
      { name: "ผู้กล้า", rank: "SSS" },
      { name: "จอมมาร", rank: "SSS" },
      { name: "ราชันย์มังกร", rank: "SSS" },
      { name: "ทูตสวรรค์", rank: "SSS" }
    ]
  };

  const stats = {
    "พลังกาย": ["G", "F", "E", "D", "C", "B", "A", "S", "SS", "SSS", "EX"],
    "พละกำลัง": ["G", "F", "E", "D", "C", "B", "A", "S", "SS", "SSS", "EX"],
    "ป้องกัน": ["G", "F", "E", "D", "C", "B", "A", "S", "SS", "SSS", "EX"],
    "ความเร็ว": ["G", "F", "E", "D", "C", "B", "A", "S", "SS", "SSS", "EX"],
    "มานา": ["G", "F", "E", "D", "C", "B", "A", "S", "SS", "SSS", "EX"],
    "พลังเวท": ["G", "F", "E", "D", "C", "B", "A", "S", "SS", "SSS", "EX"]
  };

  function generate() {
    // สุ่มสายอาชีพ
    const jobTypes = Object.keys(jobs);
    const randomJobType = jobTypes[Math.floor(Math.random() * jobTypes.length)];
    const randomJob = jobs[randomJobType][Math.floor(Math.random() * jobs[randomJobType].length)];

    // สุ่มค่าสถานะ
    let statResult = "<div class='result-box'><strong>ค่าสถานะ:</strong><br>";
    for (const stat in stats) {
      const randomStatValue = stats[stat][Math.floor(Math.random() * stats[stat].length)];
      statResult += `${stat}: ${randomStatValue}<br>`;
    }
    statResult += "</div>";

    // แสดงผล
    document.getElementById("result").innerHTML = `
      <div class='result-box'>
        <strong>สายอาชีพ:</strong> ${randomJobType}<br>
        <strong>อาชีพ:</strong> ${randomJob.name} (Rank: ${randomJob.rank})<br>
      </div>
      ${statResult}
    `;
  }
</script>

</body>
</html>
