<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AlTTS - 基于Edge TTS的语音合成工具</title>
    <style>
        :root {
            --primary-color: #007AFF;
            --secondary-color: #5856D6;
            --accent-color: #34C759;
            --bg-light: #FFFFFF;
            --text-color: #1D1D1F;
            --text-secondary: #86868B;
            --border-radius: 12px;
            --shadow: 0 8px 24px rgba(0, 0, 0, 0.08);
            --transition: all 0.3s cubic-bezier(0.4, 0.0, 0.2, 1);
            --font-family: -apple-system, BlinkMacSystemFont, 'SF Pro Text', 'SF Pro Display', system-ui, sans-serif;
        }

        body {
            font-family: var(--font-family);
            line-height: 1.47059;
            letter-spacing: -0.022em;
            background: #FBFBFD;
        }

        /* 导航栏样式 */
        .nav {
            position: sticky;
            top: 0;
            background: rgba(255, 255, 255, 0.8);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border-bottom: none;
            box-shadow: none;
            padding: 15px 0;
            z-index: 100;
            border-radius: var(--border-radius);
            margin-bottom: 30px;
        }

        .nav ul {
            list-style: none;
            padding: 0;
            margin: 0;
            display: flex;
            justify-content: center;
            gap: 20px;
            flex-wrap: wrap;
        }

        .nav a {
            color: var(--text-color);
            font-weight: 400;
            padding: 8px 16px;
            border-radius: var(--border-radius);
            transition: var(--transition);
            font-size: 12px;
            line-height: 1.33337;
            letter-spacing: -0.01em;
        }

        .nav a:hover {
            background: var(--primary-color);
            color: white;
            text-decoration: none;
        }

        /* 头部样式 */
        .header {
            text-align: center;
            margin-bottom: 40px;
            padding: 80px 40px;
            background: linear-gradient(180deg, #000000, #1D1D1F);
            color: white;
            border-radius: var(--border-radius);
            box-shadow: var(--shadow);
        }

        .header h1 {
            font-family: 'SF Pro Display', var(--font-family);
            font-size: 56px;
            line-height: 1.07143;
            font-weight: 600;
            letter-spacing: -0.005em;
            margin-bottom: 8px;
        }

        .header p {
            font-size: 28px;
            line-height: 1.10722;
            font-weight: 400;
            letter-spacing: .004em;
            color: #A1A1A6;
        }

        .logo {
            width: 120px;
            height: 120px;
            margin-bottom: 20px;
            transition: var(--transition);
        }

        .logo:hover {
            transform: scale(1.05);
        }

        /* 徽章样式 */
        .badges {
            margin: 40px 0;
            display: flex;
            justify-content: center;
            flex-wrap: wrap;
            gap: 10px;
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(20px);
            border-radius: 16px;
            padding: 24px;
        }

        .badge {
            margin: 0 5px;
            transition: var(--transition);
        }

        .badge:hover {
            transform: translateY(-2px);
        }

        /* 按钮样式 */
        .button {
            display: inline-block;
            padding: 12px 28px;
            background: var(--primary-color);
            border-radius: 980px;
            font-size: 17px;
            line-height: 1.17648;
            font-weight: 400;
            letter-spacing: -0.022em;
            border: none;
            transition: all 0.2s ease;
            color: white;
            margin: 10px;
            box-shadow: var(--shadow);
        }

        .button:hover {
            background: #0071E3;
            transform: none;
        }

        /* 预览图样式 */
        .preview {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin: 80px 0;
            flex-wrap: wrap;
        }

        .preview img {
            width: 45%;
            min-width: 300px;
            border-radius: 16px;
            box-shadow: 0 12px 32px rgba(0, 0, 0, 0.12);
            transition: var(--transition);
            border: 1px solid rgba(0, 0, 0, 0.1);
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }

        .preview img:hover {
            transform: scale(1.02);
            box-shadow: var(--shadow);
        }

        /* 功能特点样式 */
        .features {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 30px;
            margin: 40px 0;
        }

        .feature-box {
            background: rgba(255, 255, 255, 0.8);
            backdrop-filter: blur(20px);
            border: 1px solid rgba(0, 0, 0, 0.1);
            padding: 32px;
            border-radius: var(--border-radius);
            box-shadow: var(--shadow);
            transition: var(--transition);
        }

        .feature-box:hover {
            transform: translateY(-5px);
        }

        .feature-box h3 {
            font-size: 24px;
            line-height: 1.16667;
            font-weight: 600;
            letter-spacing: .009em;
            color: var(--text-color);
            margin-bottom: 16px;
        }

        .feature-box ul li {
            font-size: 17px;
            line-height: 1.47059;
            font-weight: 400;
            letter-spacing: -0.022em;
            color: var(--text-secondary);
            margin-bottom: 12px;
        }

        /* 系统要求样式 */
        .requirements {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 30px;
            margin: 40px 0;
        }

        .system-req {
            background: rgba(255, 255, 255, 0.8);
            backdrop-filter: blur(20px);
            border: 1px solid rgba(0, 0, 0, 0.1);
            padding: 25px;
            border-radius: var(--border-radius);
            box-shadow: var(--shadow);
            transition: var(--transition);
        }

        .system-req:hover {
            transform: translateX(5px);
        }

        .system-req h3 {
            font-size: 24px;
            line-height: 1.16667;
            font-weight: 600;
            letter-spacing: .009em;
            color: var(--text-color);
            margin-bottom: 16px;
        }

        .system-req ul li {
            font-size: 17px;
            line-height: 1.47059;
            font-weight: 400;
            letter-spacing: -0.022em;
            color: var(--text-secondary);
            margin-bottom: 12px;
        }

        /* 使用说明样式 */
        .usage {
            margin: 40px 0;
        }

        .usage-item {
            background: rgba(255, 255, 255, 0.8);
            backdrop-filter: blur(20px);
            border: 1px solid rgba(0, 0, 0, 0.1);
            padding: 32px;
            border-radius: var(--border-radius);
            margin-bottom: 20px;
            box-shadow: var(--shadow);
            transition: var(--transition);
        }

        .usage-item:hover {
            transform: translateX(5px);
        }

        .usage-item h3 {
            font-size: 24px;
            line-height: 1.16667;
            font-weight: 600;
            letter-spacing: .009em;
            color: var(--text-color);
            border-bottom: none;
            margin-bottom: 16px;
        }

        .usage-item ul li {
            font-size: 17px;
            line-height: 1.47059;
            font-weight: 400;
            letter-spacing: -0.022em;
            color: var(--text-secondary);
            margin-bottom: 12px;
        }

        /* 页脚样式 */
        .footer {
            text-align: center;
            margin-top: 60px;
            padding: 40px;
            background: #F5F5F7;
            color: var(--text-secondary);
            font-size: 12px;
            line-height: 1.33337;
            font-weight: 400;
            letter-spacing: -0.01em;
            border-radius: var(--border-radius);
            box-shadow: var(--shadow);
        }

        .footer a {
            color: var(--primary-color);
            text-decoration: none;
        }

        /* 返回顶部按钮 */
        #back-to-top {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background: rgba(0, 0, 0, 0.8);
            backdrop-filter: blur(20px);
            border-radius: 50%;
            width: 44px;
            height: 44px;
            font-size: 20px;
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            opacity: 0;
            transition: var(--transition);
            box-shadow: var(--shadow);
        }

        #back-to-top:hover {
            transform: translateY(-3px);
            background: #0255b3;
        }

        /* 响应式设计 */
        @media (max-width: 768px) {
            body {
                padding: 10px;
            }

            .preview img {
                width: 100%;
            }

            .nav ul {
                flex-direction: column;
                align-items: center;
            }

            .badges {
                flex-direction: column;
                align-items: center;
            }

            .header h1 {
                font-size: 40px;
            }

            .header p {
                font-size: 21px;
            }

            .feature-box, .usage-item, .system-req {
                padding: 24px;
            }
        }

        /* 添加新的动画效果 */
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

        .feature-box, .usage-item, .system-req {
            animation: fadeInUp 0.8s cubic-bezier(0.4, 0.0, 0.2, 1) forwards;
        }
    </style>
</head>
<body>
    <div class="header">
        <img src="assets/logo.png" alt="AlTTS Logo" class="logo">
        <h1>AlTTS</h1>
        <p>专业的多语言语音合成工具，基于 Microsoft Edge TTS 引擎</p>
        
        <div class="badges">
            <img class="badge" src="https://img.shields.io/badge/Platform-macOS%20%7C%20Windows-blue?style=for-the-badge" alt="Platform">
            <img class="badge" src="https://img.shields.io/badge/Python-3.12-yellow?style=for-the-badge&logo=python" alt="Python">
            <img class="badge" src="https://img.shields.io/badge/Qt-6.6.0-green?style=for-the-badge&logo=qt" alt="Qt">
            <img class="badge" src="https://img.shields.io/badge/Edge%20TTS-Latest-blue?style=for-the-badge&logo=microsoft-edge" alt="Edge TTS">
        </div>

        <div>
            <a href="https://github.com/alalalex-m/AlTTS/releases" class="button">
                <span>📥 立即下载</span>
            </a>
            <a href="#usage" class="button">
                <span>📖 使用文档</span>
            </a>
            <a href="https://github.com/alalalex-m/AlTTS/issues" class="button">
                <span>🐛 报告问题</span>
            </a>
        </div>
    </div>

    <div class="preview">
        <img src="assets/preview-light.png" alt="Light Theme">
        <img src="assets/preview-dark.png" alt="Dark Theme">
    </div>

    <div class="features">
        <div class="feature-box">
            <h3>🎯 核心功能</h3>
            <ul>
                <li>支持 70+ 种语言和方言</li>
                <li>提供 400+ 种语音音色</li>
                <li>音频参数调节</li>
                <li>字幕文件自动生成</li>
            </ul>
        </div>
        <div class="feature-box">
            <h3>🎨 界面特性</h3>
            <ul>
                <li>简洁直观的操作界面</li>
                <li>支持浅色/深色主题</li>
                <li>中英文双语界面</li>
                <li>系统主题跟随</li>
            </ul>
        </div>
    </div>

    <div class="requirements">
        <div class="system-req">
            <h3>macOS 系统要求</h3>
            <ul>
                <li>macOS 10.13 或更高版本</li>
                <li>4GB RAM</li>
                <li>100MB 可用磁盘空间</li>
                <li>稳定的网络连接</li>
                <li>Python 3.10+</li>
            </ul>
        </div>
        <div class="system-req">
            <h3>Windows 系统要求</h3>
            <ul>
                <li>Windows 10 或更高版本</li>
                <li>4GB RAM</li>
                <li>100MB 可用磁盘空间</li>
                <li>稳定的网络连接</li>
                <li>Python 3.10+</li>
            </ul>
        </div>
    </div>

    <div id="usage" class="usage">
        <h2>📖 使用说明</h2>
        <div class="usage-item">
            <h3>1️⃣ 语音选择</h3>
            <ul>
                <li>从语言列表中选择目标语言</li>
                <li>选择对应的地区和方言（如果有）</li>
                <li>选择性别（男声/女声）</li>
                <li>从筛选后的音色列表中选择</li>
            </ul>
        </div>
        <div class="usage-item">
            <h3>2️⃣ 参数调整</h3>
            <ul>
                <li>音量调节：-100% 到 +100%（建议范围：-50% 到 50%）</li>
                <li>语速调节：-100% 到 +100%（建议范围：-30% 到 30%）</li>
                <li>音高调节：-50Hz 到 +50Hz（建议范围：-20Hz 到 20Hz）</li>
                <li>实时预览：调整参数后可立即试听效果</li>
            </ul>
        </div>
        <div class="usage-item">
            <h3>3️⃣ 文本输入</h3>
            <ul>
                <li>直接在文本框中输入内容</li>
                <li>支持导入 .txt 文件</li>
                <li>支持复制粘贴操作</li>
                <li>支持文本编辑和选择</li>
            </ul>
        </div>
        <div class="usage-item">
            <h3>4️⃣ 输出选项</h3>
            <ul>
                <li>选择输出文件位置</li>
                <li>可选择是否生成字幕文件（.vtt 格式）</li>
                <li>支持预览功能</li>
                <li>显示生成进度</li>
            </ul>
        </div>
    </div>

    <div id="roadmap" class="usage">
        <h2>🚀 功能规划</h2>
        <div class="usage-item">
            <h3>当前功能</h3>
            <ul>
                <li>✅ 基础语音合成功能</li>
                <li>✅ 多语言界面支持</li>
                <li>✅ 深色模式主题</li>
                <li>✅ 字幕文件生成</li>
                <li>✅ 预览功能</li>
            </ul>
        </div>
        <div class="usage-item">
            <h3>开发计划</h3>
            <ul>
                <li>⏳ 批量处理功能</li>
                <li>⏳ 更多音频格式支持</li>
                <li>⏳ 高级音频参数设置</li>
            </ul>
        </div>
    </div>

    <div id="license" class="usage">
        <h2>⚖️ 许可说明</h2>
        <div class="usage-item">
            <p>本软件为专有软件，受版权法和国际条约保护。未经授权的复制、分发或使用本软件可能导致严重的民事和刑事处罚。</p>
            <h3>使用限制</h3>
            <ul>
                <li>禁止对软件进行修改、反编译或反向工程</li>
                <li>禁止未经授权的再分发或转授权</li>
                <li>仅限用于授权用途</li>
                <li>不提供任何形式的保证</li>
            </ul>
            <p>详细条款请查看 <a href="LICENSE">LICENSE</a> 文件。</p>
        </div>
    </div>

    <div id="contact" class="usage">
        <h2>📞 联系方式</h2>
        <div class="usage-item">
            <ul>
                <li>作者：AlxXxlA</li>
                <li>GitHub：<a href="https://github.com/alalalex-m">@alalalex-m</a></li>
                <li>抖音：<a href="https://v.douyin.com/iSgcqMFe/">@alalalex_m</a></li>
                <li>Email：<a href="mailto:alexandermcandrewog@gmail.com">alexandermcandrewog@gmail.com</a></li>
            </ul>
        </div>
    </div>

    <div class="footer">
        <p>Copyright © 2024 AlxXxlA. All rights reserved.</p>
        <div>
            <a href="https://github.com/alalalex-m">GitHub</a> ·
            <a href="https://v.douyin.com/iSgcqMFe/">抖音</a> ·
            <a href="mailto:alexandermcandrewog@gmail.com">Email</a>
        </div>
    </div>

    <!-- 返回顶部按钮 -->
    <div id="back-to-top">↑</div>

    <script>
        // 返回顶部按钮
        const backToTop = document.getElementById('back-to-top');
        window.addEventListener('scroll', () => {
            if (window.scrollY > 300) {
                backToTop.style.opacity = '1';
            } else {
                backToTop.style.opacity = '0';
            }
        });
        backToTop.addEventListener('click', () => {
            window.scrollTo({ top: 0, behavior: 'smooth' });
        });

        // 平滑滚动
        document.querySelectorAll('a[href^="#"]').forEach(anchor => {
            anchor.addEventListener('click', function (e) {
                e.preventDefault();
                document.querySelector(this.getAttribute('href')).scrollIntoView({
                    behavior: 'smooth'
                });
            });
        });

        // 添加滚动动画
        const observerOptions = {
            threshold: 0.1
        };

        const observer = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.style.opacity = "1";
                    entry.target.style.transform = "translateY(0)";
                }
            });
        }, observerOptions);

        document.querySelectorAll('.feature-box, .usage-item, .system-req').forEach(el => {
            el.style.opacity = "0";
            el.style.transform = "translateY(20px)";
            observer.observe(el);
        });
    </script>
</body>
</html> 