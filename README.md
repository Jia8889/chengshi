<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>五行定城 · 一题一页城市人格测试</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.7/dist/chart.umd.min.js">
    </script>
    <style>
        :root {
            --bg: #f3f7f4;
            --bg-soft: #eaf3ed;
            --card-bg: #ffffff;
            --text: #3d3a35;
            --text-light: #6b6560;
            --border: #dce8e1;
            --shadow: 0 2px 16px rgba(0, 0, 0, 0.04);
            --shadow-lg: 0 8px 40px rgba(0, 0, 0, 0.06);
            --gold: #c9a96e;
            --wood: #7a9a6e;
            --water: #5b8c9e;
            --fire: #c97b6b;
            --earth: #b8956a;
            --accent: #6daa82;
            --accent-hover: #4f8a62;
            --accent-light: #e6f3ea;
            --radius: 20px;
            --radius-sm: 12px;
            --transition: 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            --font: 'Segoe UI', 'PingFang SC', 'Microsoft YaHei', 'Noto Sans SC', system-ui, sans-serif;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        html {
            scroll-behavior: smooth;
            font-size: 16px;
        }
        body {
            font-family: var(--font);
            background: linear-gradient(160deg, #f0f6f3 0%, #f6faf8 30%, #eef5f1 60%, #f8fbf9 100%);
            background-attachment: fixed;
            color: var(--text);
            line-height: 1.7;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            position: relative;
        }
        body::before {
            content: '';
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background:
                radial-gradient(ellipse at 15% 10%, rgba(109, 170, 130, 0.06) 0%, transparent 55%),
                radial-gradient(ellipse at 85% 85%, rgba(91, 140, 158, 0.05) 0%, transparent 55%),
                radial-gradient(ellipse at 50% 50%, rgba(180, 200, 185, 0.04) 0%, transparent 70%);
            pointer-events: none;
            z-index: 0;
        }
        .container {
            max-width: 700px;
            width: 100%;
            padding: 0 20px 60px;
            position: relative;
            z-index: 1;
        }

        /* ========== 封面页 ========== */
        .cover-page {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 80vh;
            text-align: center;
            padding: 40px 20px;
        }
        .cover-page .cover-icons {
            display: flex;
            gap: 20px;
            margin-bottom: 28px;
            flex-wrap: wrap;
            justify-content: center;
        }
        .cover-icons span {
            width: 52px;
            height: 52px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 700;
            font-size: 1.3rem;
            color: #fff;
            box-shadow: 0 4px 16px rgba(0, 0, 0, 0.08);
            transition: transform 0.3s;
            cursor: default;
        }
        .cover-icons span:hover {
            transform: scale(1.15);
        }
        .wu-jin {
            background: #c9b99a;
        }
        .wu-mu {
            background: #8aad7a;
        }
        .wu-shui {
            background: #6a9aad;
        }
        .wu-huo {
            background: #d4917f;
        }
        .wu-tu {
            background: #c4a478;
        }
        .cover-page h1 {
            font-size: 2.5rem;
            font-weight: 800;
            color: #2c2822;
            letter-spacing: 0.04em;
            margin-bottom: 10px;
        }
        .cover-page .cover-subtitle {
            font-size: 1.08rem;
            color: #7a8e7e;
            margin-bottom: 36px;
            line-height: 1.7;
        }
        .btn-start {
            display: inline-block;
            padding: 18px 52px;
            font-size: 1.2rem;
            font-weight: 700;
            letter-spacing: 0.06em;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            background: var(--accent);
            color: #fff;
            box-shadow: 0 8px 30px rgba(109, 170, 130, 0.35);
            transition: all var(--transition);
            text-decoration: none;
        }
        .btn-start:hover {
            background: var(--accent-hover);
            box-shadow: 0 10px 36px rgba(109, 170, 130, 0.45);
            transform: translateY(-3px);
        }
        .btn-start:active {
            transform: scale(0.96);
        }

        /* ========== 进度条 & 题目区域 ========== */
        .quiz-area {
            display: none;
            animation: fadeIn 0.4s ease;
        }
        .quiz-area.visible {
            display: block;
        }
        @keyframes fadeIn {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        .progress-wrap {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 12px;
            margin: 20px 0 20px;
            font-size: 0.9rem;
            color: #7a8e7e;
            font-weight: 500;
        }
        .progress-bar {
            flex: 1;
            max-width: 300px;
            height: 8px;
            background: #dce8e1;
            border-radius: 10px;
            overflow: hidden;
        }
        .progress-fill {
            height: 100%;
            background: var(--accent);
            border-radius: 10px;
            transition: width 0.3s ease;
        }
        .question-card {
            background: var(--card-bg);
            border-radius: var(--radius);
            padding: 32px 28px;
            box-shadow: var(--shadow);
            border: 1px solid var(--border);
            margin-bottom: 20px;
            min-height: 280px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            transition: all var(--transition);
        }
        .question-num {
            font-size: 0.8rem;
            font-weight: 700;
            color: var(--accent);
            margin-bottom: 12px;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .question-num span {
            background: var(--accent-light);
            padding: 4px 12px;
            border-radius: 20px;
        }
        .question-text {
            font-size: 1.2rem;
            font-weight: 600;
            color: #2c2822;
            margin-bottom: 22px;
            line-height: 1.5;
        }
        .options-grid {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }
        .option-btn {
            display: flex;
            align-items: center;
            gap: 12px;
            padding: 14px 18px;
            border-radius: 50px;
            border: 1.5px solid #dce8e1;
            background: #f9fbf9;
            cursor: pointer;
            transition: all var(--transition);
            font-size: 0.95rem;
            color: #4a4540;
            text-align: left;
            width: 100%;
        }
        .option-btn:hover {
            background: #f0f7f2;
            border-color: #b5cfbb;
        }
        .option-btn.selected {
            background: #e6f3ea;
            border-color: var(--accent);
            font-weight: 600;
            color: #2e5a38;
            box-shadow: 0 3px 14px rgba(109, 170, 130, 0.18);
        }
        .option-letter {
            font-weight: 700;
            width: 28px;
            height: 28px;
            border-radius: 50%;
            background: #e6efe9;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 0.8rem;
            color: #7a8e7e;
            flex-shrink: 0;
        }
        .option-btn.selected .option-letter {
            background: var(--accent);
            color: #fff;
        }
        .nav-buttons {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 10px;
            flex-wrap: wrap;
            gap: 10px;
        }
        .nav-btn {
            background: #fff;
            border: 1.5px solid #c5d5c8;
            padding: 12px 24px;
            border-radius: 30px;
            cursor: pointer;
            font-weight: 600;
            font-size: 0.95rem;
            color: #4a4540;
            transition: all var(--transition);
            display: flex;
            align-items: center;
            gap: 6px;
        }
        .nav-btn:disabled {
            opacity: 0.35;
            pointer-events: none;
        }
        .nav-btn:hover:not(:disabled) {
            background: #f0f7f2;
            border-color: #8aad8e;
        }
        .btn-submit {
            background: var(--accent);
            color: #fff;
            border: none;
            padding: 14px 32px;
            border-radius: 30px;
            font-weight: 700;
            font-size: 1rem;
            cursor: pointer;
            letter-spacing: 0.04em;
            box-shadow: 0 5px 22px rgba(109, 170, 130, 0.3);
            transition: all var(--transition);
        }
        .btn-submit:hover {
            background: var(--accent-hover);
            box-shadow: 0 7px 28px rgba(109, 170, 130, 0.4);
            transform: translateY(-2px);
        }
        .btn-submit:active {
            transform: scale(0.96);
        }
        .btn-submit.skip-mode {
            background: #fff;
            color: var(--accent);
            border: 2px solid var(--accent);
            box-shadow: 0 3px 12px rgba(109, 170, 130, 0.18);
        }
        .btn-submit.skip-mode:hover {
            background: var(--accent-light);
        }

        /* ========== 结果区域 ========== */
        #result-area {
            display: none;
            margin-top: 20px;
        }
        #result-area.visible {
            display: block;
            animation: fadeInUp 0.5s ease;
        }
        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        .result-card {
            background: var(--card-bg);
            border-radius: var(--radius);
            padding: 28px 24px;
            margin-bottom: 24px;
            box-shadow: var(--shadow);
            border: 1px solid var(--border);
        }
        .result-card h3 {
            font-size: 1.2rem;
            margin-bottom: 16px;
            color: #2c2822;
            padding-left: 12px;
            border-left: 4px solid var(--accent);
        }
        .match-circle-wrap {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 24px;
            flex-wrap: wrap;
            margin-bottom: 16px;
        }
        .match-circle {
            position: relative;
            width: 120px;
            height: 120px;
        }
        .match-circle svg {
            transform: rotate(-90deg);
        }
        .match-percent {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 1.6rem;
            font-weight: 800;
            color: #3d3a35;
        }
        .grid-9 {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            margin-top: 10px;
        }
        .grid-item {
            background: #f9fbf9;
            border-radius: var(--radius-sm);
            padding: 14px 6px;
            text-align: center;
            font-weight: 600;
            font-size: 0.9rem;
            color: #4a4540;
            border: 1px solid #e6efe9;
        }
        .backup-cities {
            display: flex;
            flex-wrap: wrap;
            gap: 12px;
            margin-top: 10px;
        }
        .backup-city {
            flex: 1 1 150px;
            background: #f9fbf9;
            border-radius: var(--radius-sm);
            padding: 16px;
            text-align: center;
            border: 1px solid #dce8e1;
        }
        .backup-city .city-name {
            font-weight: 700;
            font-size: 1rem;
        }
        .advice-text {
            font-size: 0.92rem;
            color: #4a4540;
            line-height: 1.8;
        }
        .personality-card {
            background: linear-gradient(135deg, #f5faf7 0%, #eef5f1 100%);
            border: 2px solid #c5d5c8;
            border-radius: var(--radius);
            padding: 28px;
            text-align: center;
        }
        .personality-card .city-title {
            font-size: 2rem;
            font-weight: 800;
            color: #2c2822;
        }
        .personality-card .wu-label {
            font-size: 0.9rem;
            color: #6b8a72;
            background: #fff;
            display: inline-block;
            padding: 4px 18px;
            border-radius: 20px;
            margin: 8px 0;
        }
        .personality-card .motto {
            font-style: italic;
            color: #5c554b;
            margin-top: 8px;
            font-size: 1rem;
        }
        .incomplete-badge {
            display: inline-block;
            background: #fef3e4;
            color: #b8783e;
            font-size: 0.8rem;
            padding: 4px 12px;
            border-radius: 14px;
            margin-left: 8px;
            font-weight: 500;
        }

        @media (max-width: 600px) {
            .cover-page h1 {
                font-size: 1.9rem;
            }
            .cover-page .cover-subtitle {
                font-size: 0.9rem;
            }
            .btn-start {
                padding: 15px 36px;
                font-size: 1rem;
            }
            .question-card {
                padding: 20px 16px;
            }
            .question-text {
                font-size: 1.05rem;
            }
            .option-btn {
                font-size: 0.85rem;
                padding: 11px 14px;
                gap: 8px;
            }
            .option-letter {
                width: 24px;
                height: 24px;
                font-size: 0.7rem;
            }
            .nav-btn {
                padding: 10px 16px;
                font-size: 0.85rem;
            }
            .btn-submit {
                padding: 12px 24px;
                font-size: 0.9rem;
            }
            .match-circle {
                width: 90px;
                height: 90px;
            }
            .match-circle svg {
                width: 90px;
                height: 90px;
            }
            .match-percent {
                font-size: 1.3rem;
            }
            .grid-9 {
                grid-template-columns: repeat(3, 1fr);
                gap: 6px;
            }
            .grid-item {
                font-size: 0.78rem;
                padding: 10px 4px;
            }
            .personality-card .city-title {
                font-size: 1.5rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- ==================== 封面页 ==================== -->
        <div class="cover-page" id="cover-page">
            <div class="cover-icons">
                <span class="wu-jin">金</span>
                <span class="wu-mu">木</span>
                <span class="wu-shui">水</span>
                <span class="wu-huo">火</span>
                <span class="wu-tu">土</span>
            </div>
            <h1>五行定城</h1>
            <p class="cover-subtitle">
                25道题目 · 探寻你的灵魂城市<br>
                匹配度 · 九宫格 · 四维雷达 · 城世人格卡
            </p>
            <button class="btn-start" id="btn-start">✨ 开启测试</button>
        </div>

        <!-- ==================== 测试区域 ==================== -->
        <div class="quiz-area" id="quiz-area">
            <div class="progress-wrap" id="progress-wrap">
                <span id="progress-text">1 / 25</span>
                <div class="progress-bar"><div class="progress-fill" id="progress-fill" style="width:4%;"></div></div>
                <span id="completion-hint" style="font-size:0.8rem;color:#7a8e7e;">已答 0 题</span>
            </div>

            <div class="question-card" id="question-card">
                <div class="question-num"><span id="q-num-label">第 1 题</span></div>
                <div class="question-text" id="question-text"></div>
                <div class="options-grid" id="options-container"></div>
            </div>

            <div class="nav-buttons">
                <button class="nav-btn" id="prev-btn" disabled>← 上一题</button>
                <button class="btn-submit" id="submit-btn">✨ 提交 · 查看结果</button>
                <button class="nav-btn" id="next-btn">下一题 →</button>
            </div>
        </div>

        <!-- ==================== 结果区域 ==================== -->
        <div id="result-area">
            <div class="result-card" id="score-card">
                <h3>📊 五行能量分布 <span id="incomplete-badge" class="incomplete-badge" style="display:none;">部分题目未作答</span></h3>
                <div id="score-grid" style="display:flex;gap:10px;flex-wrap:wrap;justify-content:center;"></div>
                <div style="max-width:380px;margin:16px auto;"><canvas id="radar-chart"></canvas></div>
            </div>
            <div class="result-card" id="match-card">
                <h3>🏆 最匹配城市</h3>
                <div class="match-circle-wrap">
                    <div class="match-circle">
                        <svg width="120" height="120" viewBox="0 0 120 120">
                            <circle cx="60" cy="60" r="50" stroke="#dce8e1" stroke-width="8" fill="none"/>
                            <circle id="match-circle-fg" cx="60" cy="60" r="50" stroke="#6daa82" stroke-width="8" fill="none" stroke-dasharray="314" stroke-dashoffset="314"/>
                        </svg>
                        <div class="match-percent" id="match-percent-text">92%</div>
                    </div>
                    <div>
                        <h2 id="best-city-name">上海</h2>
                        <p id="wu-element-desc" style="color:#6b8a72;">金行 · 秩序与效率</p>
                    </div>
                </div>
                <div id="match-reason" style="font-size:0.92rem;color:#4a4540;line-height:1.7;"></div>
            </div>
            <div class="result-card">
                <h3>🧩 城市人格九宫格</h3>
                <div class="grid-9" id="grid-9"></div>
            </div>
            <div class="result-card">
                <h3>📐 四维度适配雷达</h3>
                <div style="max-width:380px;margin:0 auto;"><canvas id="radar-4d"></canvas></div>
            </div>
            <div class="result-card">
                <h3>🔁 三个备选城市</h3>
                <div class="backup-cities" id="backup-cities"></div>
            </div>
            <div class="result-card">
                <h3>💡 城市建议</h3>
                <div class="advice-text" id="advice-text"></div>
            </div>
            <div class="result-card personality-card" id="personality-card">
                <p style="color:#7a8e7e;font-size:0.8rem;">专属城世人格卡</p>
                <div class="city-title" id="pc-city">上海</div>
                <div class="wu-label" id="pc-wu">金 · 秩序之美</div>
                <p class="motto" id="pc-motto">"你与这座城市的天际线共鸣"</p>
                <p style="font-size:0.75rem;color:#7a8e7e;margin-top:12px;" id="pc-date"></p>
            </div>
        </div>
    </div>

    <script>
        (function() {
            const TOTAL_QUESTIONS = 25;
            const questions = [
                { text: '你理想中的居住环境是？', options: ['A.高楼现代商务区', 'B.绿树花园洋房', 'C.临湖水景住宅', 'D.霓虹热闹市中心',
                        'E.青砖古树历史街区'
                    ] },
                { text: '周末你更愿意怎样度过？', options: ['A.科技展/艺术拍卖', 'B.郊野登山赏花', 'C.湖边骑行/划船', 'D.音乐节/演唱会',
                    'E.博物馆/古城淘旧物'] },
                { text: '你最欣赏自己或他人的哪种特质？', options: ['A.果断有效率逻辑清晰', 'B.仁爱包容温和生长', 'C.智慧随机应变深沉', 'D.热情爆发力感染力',
                        'E.诚信稳重承载万物'
                    ] },
                { text: '一年中最让你感到舒适的季节是？', options: ['A.秋天天高云淡清爽', 'B.春天万物复苏和风', 'C.冬天白雪沉静内藏', 'D.夏天骄阳生命怒放',
                        'E.长夏土地丰饶平和'] },
                { text: '凭直觉，以下哪个色系最能代表你？', options: ['A.白/银/金属光泽', 'B.青/绿/植物嫩芽', 'C.黑/深蓝/海天色', 'D.红/紫/烈火霓虹',
                        'E.黄/棕/大地颜色'
                    ] },
                { text: '以下哪种职业方向对你有天然的吸引力？', options: ['A.金融/律师/精密工程', 'B.农艺/教师/中医环保', 'C.航海贸易/心理咨询',
                        'D.导演/厨师/互联网创业', 'E.建筑师/考古/公务员'] },
                { text: '如果获得一张免费机票，你更想去？', options: ['A.上海陆家嘴天际线', 'B.张家界峰林漫步', 'C.三亚枕着海浪入眠', 'D.重庆穿楼轻轨不夜火锅',
                        'E.西安城墙骑行看兵马俑'] },
                { text: '在社交场合，你更接近哪种状态？', options: ['A.组织者把控节奏重边界', 'B.倾听者照顾他人如树可靠', 'C.观察者如水融入灵活切换',
                        'D.气氛带动者点燃全场', 'E.协调者不偏不倚踏实安全'] },
                { text: '当你感到压力很大时，你的第一反应是？', options: ['A.隔离情绪冷静分析列计划', 'B.找朋友倾诉自然中散步', 'C.顺其自然放空旅行抽离',
                        'D.运动呐喊甚至发泄出来', 'E.默默承受慢慢消化'] },
                { text: '哪种价值观最能让你感到生命的意义？', options: ['A.公正卓越不断精进', 'B.仁爱共生和谐之美', 'C.智慧自由灵性觉醒', 'D.荣誉激情闪耀瞬间',
                        'E.诚信传承家园稳固'] },
                { text: '哪一种气息会让你瞬间放松？', options: ['A.雨后金属味/新书油墨香', 'B.雨后森林泥土芬芳青草香', 'C.海风湿咸水汽湖边湿润空气',
                        'D.篝火烧烤烟熏热烈气息', 'E.旧书陈纸味/黄土阳光味'] },
                { text: '你的口味和饮食习惯偏向？', options: ['A.精致料理讲究清淡', 'B.偏好素食新鲜蔬果', 'C.酷爱海鲜汤羹茶饮', 'D.无辣不欢煎炸烧烤',
                    'E.面食谷物家常炖菜香浓'] },
                { text: '听到哪种音乐类型，你最容易起鸡皮疙瘩？', options: ['A.严谨古典乐/冷冽电子', 'B.森林民谣/新世纪自然音乐', 'C.慵懒爵士/故事感蓝调',
                        'D.力量摇滚/躁动电子舞曲', 'E.醇厚民族音乐/史诗交响'] },
                { text: '你更喜欢或更擅长的运动方式？', options: ['A.击剑射击攀岩瞬间判断', 'B.瑜伽徒步园艺舒展生长', 'C.游泳潜水冲浪与水共舞',
                        'D.篮球搏击短跑爆发性', 'E.太极举重稳定长距离步行'] },
                { text: '若要养一只宠物，你倾向于？', options: ['A.飞鸟或猫独立优雅', 'B.兔子小鹿温和植食', 'C.鱼类或龟安静灵动', 'D.狗热情忠诚充满活力',
                    'E.陆龟或仓鼠安稳敦厚'] },
                { text: '你的思维方式更偏向？', options: ['A.逻辑推理数理因果', 'B.联想创新捕捉灵感图像', 'C.直觉洞察抽象隐喻', 'D.行动实践快速反应',
                    'E.经验归纳稳扎稳打'] },
                { text: '在一个项目团队中，你本能地去承担哪种角色？', options: ['A.推进者制定时间表果断决策', 'B.凝合剂关心成员鼓舞士气', 'C.谋士出点子灵活应对',
                        'D.尖兵冲在最前面执行力强', 'E.定心丸保障后勤维持稳定'] },
                { text: '以下哪种天气最让你身心舒畅？', options: ['A.秋高气爽天空湛蓝落叶', 'B.春风和煦柳絮细雨', 'C.冬雪纷飞天地素白万籁寂',
                        'D.夏日炎炎蝉鸣热浪滚滚', 'E.微风多云不湿不燥平稳'] },
                { text: '哪种光影氛围最能让你产生灵感或安宁？', options: ['A.锐利明亮日光灯现代冷光', 'B.树叶斑驳光影晨曦暖光', 'C.湖海波光粼粼流转水纹光',
                        'D.城市霓虹篝火跳动火星', 'E.柔和昏黄烛光旧式暖黄光晕'] },
                { text: '以下哪种景观最能震撼你的心灵？', options: ['A.摩天大楼钢铁天际线', 'B.层峦叠嶂原始森林', 'C.奔流江河壮阔海洋', 'D.火山熔岩盛大烟火',
                    'E.辽阔大漠风吹草低草原'] },
                { text: '如果把自己比喻成一本书，你更希望它是？', options: ['A.冷静工具书/百科全书', 'B.诗意散文诗集', 'C.哲学随笔/悬疑小说', 'D.热血励志冒险小说',
                        'E.厚重严肃历史传记'] },
                { text: '面对一个棘手的选择，你的决策风格更接近？', options: ['A.深思熟虑用数据权衡利弊', 'B.听从内心感觉跟着直觉走', 'C.灵活变动见招拆招不行就换',
                        'D.冲动且果断先做了再说', 'E.参照前人经验选最稳妥路径'] },
                { text: '假如可以拥有一种超能力，你最想要？', options: ['A.金属控制/点石成金', 'B.与植物对话/瞬间治愈', 'C.控制水流/隐身于雾',
                        'D.操纵火焰/御空飞行', 'E.掌控大地/移山填海'] },
                { text: '对你来说，哪种学习方式效率最高？', options: ['A.系统化课程严格知识架构', 'B.故事图像联想在自然中学习', 'C.自由讨论碰撞观点如水吸收',
                        'D.实践出真知比赛试错掌握', 'E.反复记诵详尽笔记夯实基础'] },
                { text: '你对未来人生的终极憧憬是？', options: ['A.功成名就专业领域话语权财富', 'B.世外桃源与自然所爱之人共生', 'C.环游世界体验千万种人生',
                        'D.轰轰烈烈燃烧让世界记住我', 'E.家园安稳家族兴旺如大地敦实'] }
            ];

            const OPTION_MAP = ['A', 'B', 'C', 'D', 'E'];
            const ELEMENT_MAP = { 'A': '金', 'B': '木', 'C': '水', 'D': '火', 'E': '土' };
            const ELEMENTS = ['金', '木', '水', '火', '土'];

            const cityDataMap = {
                '金': {
                    bestCity: '上海',
                    matchPercent: 92,
                    wuDesc: '金行 · 秩序与效率',
                    matchReason: '上海是全球金融中心，天际线锐利、节奏明快，极其契合金行人格的果断、逻辑与秩序感。陆家嘴的玻璃幕墙与外滩的百年建筑，映射出你内心对卓越与规则的追求。',
                    grid: ['现代', '精致', '高效', '金融', '开放', '时尚', '机遇', '规则', '国际'],
                    radar4D: [70, 85, 95, 80],
                    backupCities: ['深圳', '苏州', '香港'],
                    backupReasons: { '深圳': '创新引擎，年轻而锋利', '苏州': '园林精工，古典与现代的精妙平衡',
                        '香港': '国际都会，高效与繁华的极致' },
                    advice: '选择高层公寓，保持视野开阔；职业倾向金融、法律、精密工程；定期旅行至海滨或温泉，以水柔化金的刚硬。',
                    personalityCard: { city: '上海', wu: '金 · 秩序之美', motto: '你与这座城市的天际线共鸣' }
                },
                '木': {
                    bestCity: '杭州',
                    matchPercent: 95,
                    wuDesc: '木行 · 生长与诗意',
                    matchReason: '杭州湖山相映，龙井茶香、灵隐钟声、西溪芦雪，木行的仁爱、包容与创造力在此得到最舒展的生长。这座城市允许你慢下来，在自然节律中呼吸。',
                    grid: ['诗意', '自然', '茶韵', '创新', '生态', '温润', '艺术', '慢生活', '灵动'],
                    radar4D: [95, 75, 85, 95],
                    backupCities: ['成都', '昆明', '厦门'],
                    backupReasons: { '成都': '天府之国，巴适安逸的生态慢城', '昆明': '春城花都，四季如春的生机之域',
                        '厦门': '海上花园，文艺清新的海岛木系' },
                    advice: '选择有阳台或小院的居所，种植绿植；职业倾向教育、园艺、艺术；每年至少一次森林浴之旅。',
                    personalityCard: { city: '杭州', wu: '木 · 诗意生长', motto: '在湖山之间，找到你的生命节律' }
                },
                '水': {
                    bestCity: '青岛',
                    matchPercent: 93,
                    wuDesc: '水行 · 灵动与深邃',
                    matchReason: '青岛三面环海，红瓦绿树、碧海蓝天，水行所需的流动、智慧与精神留白在此完美满足。栈桥观海、老城漫步，潮汐带来无尽的灵感。',
                    grid: ['辽阔', '清新', '浪漫', '海洋', '活力', '文艺', '宜居', '开放', '雅致'],
                    radar4D: [90, 80, 80, 85],
                    backupCities: ['大连', '三亚', '珠海'],
                    backupReasons: { '大连': '浪漫之都，北方的蓝色港湾', '三亚': '极致水元素，热带海洋的治愈场',
                        '珠海': '百岛之市，宁静与开阔并存' },
                    advice: '居所务必近水；家中布置鱼缸与蓝色调；职业适合创意、咨询、贸易；常去海边充电。',
                    personalityCard: { city: '青岛', wu: '水 · 灵动深邃', motto: '你的灵魂，是一片蔚蓝的潮汐' }
                },
                '火': {
                    bestCity: '重庆',
                    matchPercent: 91,
                    wuDesc: '火行 · 热情与爆发',
                    matchReason: '重庆的8D魔幻、沸腾火锅、洪崖洞夜景，完美点燃火行的热情与行动力。这座城市从不冷场，每一条坡道都在诉说热辣的故事。',
                    grid: ['热辣', '魔幻', '江湖', '夜生活', '豪爽', '创新', '美食', '立体', '沸腾'],
                    radar4D: [65, 90, 85, 90],
                    backupCities: ['长沙', '武汉', '南京'],
                    backupReasons: { '长沙': '不夜星城，烟火人间的热血江湖', '武汉': '九省通衢，江湖交汇的活力枢纽',
                        '南京': '金陵古都，热情与底蕴交融' },
                    advice: '选择热闹便利的社区；职业倾向创业、演艺、餐饮；注意用水行活动（游泳）平衡情绪。',
                    personalityCard: { city: '重庆', wu: '火 · 热烈沸腾', motto: '你的生命，本就是一场璀璨烟火' }
                },
                '土': {
                    bestCity: '北京',
                    matchPercent: 90,
                    wuDesc: '土行 · 厚重与承载',
                    matchReason: '北京方正稳重，故宫的红墙黄瓦、胡同的青砖灰瓦，土行的诚信、传承与安稳在此扎根。三千年古都的气场，给你最深的归属感。',
                    grid: ['厚重', '皇城', '大气', '历史', '包容', '机遇', '文化', '规矩', '踏实'],
                    radar4D: [60, 70, 90, 95],
                    backupCities: ['洛阳', '西安', '济南'],
                    backupReasons: { '洛阳': '神都故地，十三朝沉淀的厚土', '西安': '长安遗韵，城墙上吹过千年的风',
                        '济南': '泉城温厚，中庸而安稳' },
                    advice: '选择安静稳定的老城区；职业适合建筑、考古、公务员；多接触绿植防止思维僵化。',
                    personalityCard: { city: '北京', wu: '土 · 厚德载物', motto: '你的根基，深植于千年的土壤' }
                }
            };

            let currentIndex = 0;
            let answers = new Array(TOTAL_QUESTIONS).fill(null);
            let radarChartInstance = null;
            let radar4DInstance = null;
            let isStarted = false;

            // DOM
            const coverPage = document.getElementById('cover-page');
            const quizArea = document.getElementById('quiz-area');
            const btnStart = document.getElementById('btn-start');
            const questionCard = document.getElementById('question-card');
            const qNumLabel = document.getElementById('q-num-label');
            const questionTextEl = document.getElementById('question-text');
            const optionsContainer = document.getElementById('options-container');
            const prevBtn = document.getElementById('prev-btn');
            const nextBtn = document.getElementById('next-btn');
            const submitBtn = document.getElementById('submit-btn');
            const progressText = document.getElementById('progress-text');
            const progressFill = document.getElementById('progress-fill');
            const completionHint = document.getElementById('completion-hint');
            const resultArea = document.getElementById('result-area');
            const incompleteBadge = document.getElementById('incomplete-badge');

            function updateProgress() {
                const answeredCount = answers.filter(a => a !== null).length;
                progressText.textContent = `${currentIndex + 1} / ${TOTAL_QUESTIONS}`;
                const percent = (answeredCount / TOTAL_QUESTIONS) * 100;
                progressFill.style.width = percent + '%';
                const allDone = answeredCount === TOTAL_QUESTIONS;
                completionHint.textContent = allDone ? '✅ 已完成全部题目' : `已答 ${answeredCount} 题`;
                completionHint.style.color = allDone ? '#6daa82' : '#7a8e7e';
            }

            function renderQuestion() {
                if (!isStarted) return;
                const q = questions[currentIndex];
                qNumLabel.textContent = `第 ${currentIndex + 1} 题`;
                questionTextEl.textContent = q.text;
                optionsContainer.innerHTML = '';
                const selectedAnswer = answers[currentIndex];

                q.options.forEach((optText, idx) => {
                    const letter = OPTION_MAP[idx];
                    const btn = document.createElement('div');
                    btn.className = 'option-btn';
                    if (selectedAnswer === letter) btn.classList.add('selected');
                    btn.innerHTML =
                        `<span class="option-letter">${letter}</span><span>${optText}</span>`;
                    btn.addEventListener('click', () => {
                        answers[currentIndex] = letter;
                        renderQuestion();
                        updateProgress();
                        updateNavButtons();
                        if (currentIndex < TOTAL_QUESTIONS - 1) {
                            setTimeout(() => {
                                currentIndex++;
                                renderQuestion();
                                updateProgress();
                                updateNavButtons();
                            }, 300);
                        }
                    });
                    optionsContainer.appendChild(btn);
                });

                updateNavButtons();
                updateProgress();
            }

            function updateNavButtons() {
                if (!isStarted) return;
                const allAnswered = answers.every(a => a !== null);
                prevBtn.disabled = currentIndex === 0;

                // 提交按钮始终可见且可用
                submitBtn.style.display = 'inline-block';
                submitBtn.disabled = false;

                if (allAnswered) {
                    submitBtn.classList.remove('skip-mode');
                    submitBtn.textContent = '✨ 提交 · 查看结果';
                } else {
                    submitBtn.classList.add('skip-mode');
                    const remaining = TOTAL_QUESTIONS - answers.filter(a => a !== null).length;
                    submitBtn.textContent = `⚡ 跳过 ${remaining} 题 · 查看结果`;
                }

                if (currentIndex === TOTAL_QUESTIONS - 1) {
                    nextBtn.style.display = 'none';
                } else {
                    nextBtn.style.display = 'inline-flex';
                }
            }

            function goToPrev() {
                if (currentIndex > 0) {
                    currentIndex--;
                    renderQuestion();
                }
            }

            function goToNext() {
                if (currentIndex < TOTAL_QUESTIONS - 1) {
                    currentIndex++;
                    renderQuestion();
                }
            }

            // 为未答题目随机填充答案
            function fillMissingAnswers() {
                const options = ['A', 'B', 'C', 'D', 'E'];
                for (let i = 0; i < TOTAL_QUESTIONS; i++) {
                    if (answers[i] === null) {
                        answers[i] = options[Math.floor(Math.random() * options.length)];
                    }
                }
            }

            // 开始测试
            btnStart.addEventListener('click', () => {
                isStarted = true;
                coverPage.style.display = 'none';
                quizArea.classList.add('visible');
                renderQuestion();
                updateNavButtons();
                questionCard.scrollIntoView({ behavior: 'smooth', block: 'center' });
            });

            prevBtn.addEventListener('click', goToPrev);
            nextBtn.addEventListener('click', goToNext);
            submitBtn.addEventListener('click', handleSubmit);

            document.addEventListener('keydown', (e) => {
                if (!isStarted) return;
                if (e.key === 'ArrowLeft' && !prevBtn.disabled) goToPrev();
                if (e.key === 'ArrowRight' && currentIndex < TOTAL_QUESTIONS - 1) goToNext();
                if (e.key === 'Enter') handleSubmit();
            });

            function calculateScores() {
                const scores = { '金': 0, '木': 0, '水': 0, '火': 0, '土': 0 };
                answers.forEach(ans => {
                    if (ans) scores[ELEMENT_MAP[ans]]++;
                });
                return scores;
            }

            function getPrimaryElement(scores) {
                const max = Math.max(...Object.values(scores));
                return ELEMENTS.find(el => scores[el] === max);
            }

            function handleSubmit() {
                const wasIncomplete = answers.some(a => a === null);
                if (wasIncomplete) {
                    fillMissingAnswers();
                }
                const scores = calculateScores();
                const primary = getPrimaryElement(scores);
                const cityData = cityDataMap[primary];

                resultArea.classList.add('visible');
                resultArea.style.display = 'block';

                // 显示是否部分未答
                incompleteBadge.style.display = wasIncomplete ? 'inline-block' : 'none';

                setTimeout(() => {
                    renderResults(scores, primary, cityData);
                    resultArea.scrollIntoView({ behavior: 'smooth', block: 'start' });
                }, 100);
            }

            function renderResults(scores, primary, cityData) {
                // 五行得分
                const scoreGrid = document.getElementById('score-grid');
                scoreGrid.innerHTML = '';
                ELEMENTS.forEach(el => {
                    const div = document.createElement('div');
                    div.style.cssText =
                        `text-align:center;padding:8px 12px;border-radius:12px;background:#f9fbf9;border:1px solid #dce8e1;`;
                    if (el === primary) div.style.borderColor = '#6daa82';
                    div.innerHTML =
                        `<div style="font-size:1.5rem;font-weight:700;">${scores[el]}</div><div style="font-size:0.8rem;color:#7a8e7e;">${el}</div>`;
                    scoreGrid.appendChild(div);
                });

                // 五行雷达
                if (radarChartInstance) radarChartInstance.destroy();
                const ctx = document.getElementById('radar-chart').getContext('2d');
                const dataValues = ELEMENTS.map(el => scores[el]);
                radarChartInstance = new Chart(ctx, {
                    type: 'radar',
                    data: {
                        labels: ELEMENTS,
                        datasets: [{
                            label: '五行能量',
                            data: dataValues,
                            backgroundColor: 'rgba(109,170,130,0.12)',
                            borderColor: '#6daa82',
                            borderWidth: 2,
                            pointBackgroundColor: '#6daa82',
                            pointRadius: 5,
                        }]
                    },
                    options: {
                        scales: { r: { beginAtZero: true, max: Math.max(...dataValues, 5) + 2,
                                ticks: { stepSize: 1 } } },
                        plugins: { legend: { display: false } }
                    }
                });

                // 匹配城市
                document.getElementById('best-city-name').textContent = cityData.bestCity;
                document.getElementById('wu-element-desc').textContent = cityData.wuDesc;
                document.getElementById('match-reason').textContent = cityData.matchReason;
                const circle = document.getElementById('match-circle-fg');
                const circumference = 2 * Math.PI * 50;
                const offset = circumference - (cityData.matchPercent / 100) * circumference;
                circle.setAttribute('stroke-dasharray', circumference);
                circle.setAttribute('stroke-dashoffset', offset);
                document.getElementById('match-percent-text').textContent = cityData.matchPercent + '%';

                // 九宫格
                const grid9 = document.getElementById('grid-9');
                grid9.innerHTML = '';
                cityData.grid.forEach(word => {
                    const item = document.createElement('div');
                    item.className = 'grid-item';
                    item.textContent = word;
                    grid9.appendChild(item);
                });

                // 四维雷达
                if (radar4DInstance) radar4DInstance.destroy();
                const ctx4d = document.getElementById('radar-4d').getContext('2d');
                radar4DInstance = new Chart(ctx4d, {
                    type: 'radar',
                    data: {
                        labels: ['自然环境', '生活节奏', '职业发展', '人文氛围'],
                        datasets: [{
                            label: cityData.bestCity + ' 适配度',
                            data: cityData.radar4D,
                            backgroundColor: 'rgba(201,169,110,0.18)',
                            borderColor: '#c9a96e',
                            borderWidth: 2,
                            pointBackgroundColor: '#c9a96e',
                            pointRadius: 5,
                        }]
                    },
                    options: {
                        scales: { r: { beginAtZero: true, max: 100, ticks: { stepSize: 20 } } },
                        plugins: { legend: { display: true, position: 'bottom' } }
                    }
                });

                // 备选城市
                const backupDiv = document.getElementById('backup-cities');
                backupDiv.innerHTML = '';
                cityData.backupCities.forEach(city => {
                    const div = document.createElement('div');
                    div.className = 'backup-city';
                    div.innerHTML =
                        `<div class="city-name">${city}</div><div style="font-size:0.8rem;color:#7a8e7e;">${cityData.backupReasons[city]}</div>`;
                    backupDiv.appendChild(div);
                });

                // 建议
                document.getElementById('advice-text').textContent = cityData.advice;

                // 城世人格卡
                document.getElementById('pc-city').textContent = cityData.personalityCard.city;
                document.getElementById('pc-wu').textContent = cityData.personalityCard.wu;
                document.getElementById('pc-motto').textContent = '"' + cityData.personalityCard.motto + '"';
                document.getElementById('pc-date').textContent = new Date().toLocaleDateString('zh-CN', {
                    year: 'numeric',
                    month: 'long',
                    day: 'numeric'
                }) + ' · 五行定城';
            }
        })();
    </script>
</body>
</html>
