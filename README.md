# codigocasa-a-dois[Código Casa a Dois - Organização de Tarefas.html](https://github.com/user-attachments/files/22085463/Codigo.Casa.a.Dois.-.Organizacao.de.Tarefas.html)

<!-- saved from url=(0043)file:///C:/Users/migue/Downloads/index.html -->
<html lang="pt-BR"><head><meta http-equiv="Content-Type" content="text/html; charset=UTF-8"><script type="text/javascript">
        var gk_isXlsx = false;
        var gk_xlsxFileLookup = {};
        var gk_fileData = {};
        function filledCell(cell) {
          return cell !== '' && cell != null;
        }
        function loadFileData(filename) {
        if (gk_isXlsx && gk_xlsxFileLookup[filename]) {
            try {
                var workbook = XLSX.read(gk_fileData[filename], { type: 'base64' });
                var firstSheetName = workbook.SheetNames[0];
                var worksheet = workbook.Sheets[firstSheetName];

                // Convert sheet to JSON to filter blank rows
                var jsonData = XLSX.utils.sheet_to_json(worksheet, { header: 1, blankrows: false, defval: '' });
                // Filter out blank rows (rows where all cells are empty, null, or undefined)
                var filteredData = jsonData.filter(row => row.some(filledCell));

                // Heuristic to find the header row by ignoring rows with fewer filled cells than the next row
                var headerRowIndex = filteredData.findIndex((row, index) =>
                  row.filter(filledCell).length >= filteredData[index + 1]?.filter(filledCell).length
                );
                // Fallback
                if (headerRowIndex === -1 || headerRowIndex > 25) {
                  headerRowIndex = 0;
                }

                // Convert filtered JSON back to CSV
                var csv = XLSX.utils.aoa_to_sheet(filteredData.slice(headerRowIndex)); // Create a new sheet from filtered array of arrays
                csv = XLSX.utils.sheet_to_csv(csv, { header: 1 });
                return csv;
            } catch (e) {
                console.error(e);
                return "";
            }
        }
        return gk_fileData[filename] || "";
        }
        </script>


    
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Código Casa a Dois - Organização de Tarefas</title>
    <style>
        :root {
            --primary: #dc2626;
            --primary-dark: #991b1b;
            --accent1: #ec4899; /* pinkish for partner1 */
            --accent2: #60a5fa; /* light blue for partner2 */
            --accent3: #22c55e; /* green for shared */
            --bg-light: #f8fafc;
            --bg-white: #ffffff;
            --text-dark: #1f2937;
            --text-muted: #6b7280;
            --shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -2px rgba(0, 0, 0, 0.05);
            --transition: all 0.3s ease;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background: linear-gradient(135deg, var(--bg-light) 0%, #e2e8f0 100%);
            min-height: 100vh;
            padding: 1rem;
            font-size: clamp(16px, 2.5vw, 18px);
            color: var(--text-dark);
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            background: var(--bg-white);
            border-radius: 1rem;
            box-shadow: var(--shadow);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, var(--primary) 0%, var(--primary-dark) 100%);
            padding: 2rem;
            text-align: center;
            color: var(--bg-white);
        }

        .header h1 {
            font-size: clamp(1.8rem, 5vw, 2.5rem);
            font-weight: 700;
            margin-bottom: 0.5rem;
        }

        .header p {
            font-size: clamp(1rem, 2.5vw, 1.2rem);
            opacity: 0.9;
        }

        .tabs {
            display: flex;
            flex-wrap: wrap;
            background: var(--bg-white);
            border-bottom: 1px solid #e5e7eb;
            position: relative;
        }

        .tab {
            flex: 1;
            padding: 1rem;
            background: transparent;
            border: none;
            cursor: pointer;
            font-size: clamp(0.9rem, 2vw, 1rem);
            font-weight: 500;
            color: var(--text-muted);
            transition: var(--transition);
            text-align: center;
            white-space: nowrap;
        }

        .tab.active {
            color: var(--primary);
            border-bottom: 3px solid var(--primary);
        }

        .tab:hover, .tab:focus {
            background: #f1f5f9;
            outline: none;
        }

        .content {
            display: none;
            padding: 2rem;
        }

        .content.active {
            display: block;
        }

        .couple-setup {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 1.5rem;
            margin-bottom: 2rem;
        }

        .partner {
            background: var(--bg-white);
            padding: 1.5rem;
            border-radius: 0.75rem;
            border: 1px solid #e5e7eb;
            transition: var(--transition);
        }

        .partner:hover, .partner:focus-within {
            border-color: var(--primary);
            transform: translateY(-2px);
            box-shadow: var(--shadow);
        }

        .partner h3 {
            color: var(--primary);
            margin-bottom: 1rem;
            font-size: 1.25rem;
        }

        .partner input {
            width: 100%;
            padding: 0.75rem;
            border: 1px solid #d1d5db;
            border-radius: 0.5rem;
            font-size: 1rem;
            margin-bottom: 0.75rem;
            transition: var(--transition);
        }

        .partner input:focus {
            border-color: var(--primary);
            outline: none;
            box-shadow: 0 0 0 3px rgba(220, 38, 38, 0.1);
        }

        .tasks-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 1.5rem;
            margin-top: 2rem;
        }

        .task-category {
            background: var(--bg-white);
            border-radius: 0.75rem;
            padding: 1.5rem;
            border: 1px solid #e5e7eb;
            box-shadow: var(--shadow);
        }

        .task-category h4 {
            color: var(--primary);
            margin-bottom: 1rem;
            font-size: 1.1rem;
            text-align: center;
        }

        .task-item {
            background: var(--bg-light);
            padding: 1rem;
            border-radius: 0.5rem;
            margin-bottom: 0.75rem;
            border: 1px solid #e5e7eb;
            cursor: pointer;
            transition: var(--transition);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .task-item:hover {
            border-color: var(--primary);
            transform: translateX(3px);
        }

        .task-item.assigned-partner1 {
            border-color: var(--accent1);
            background: linear-gradient(145deg, #fdf2f8, #fce7f3);
        }

        .task-item.assigned-partner2 {
            border-color: var(--accent2);
            background: linear-gradient(145deg, #eff6ff, #dbeafe);
        }

        .task-item.shared {
            border-color: var(--accent3);
            background: linear-gradient(145deg, #f0fdf4, #dcfce7);
        }

        .task-controls {
            display: flex;
            gap: 0.5rem;
        }

        .assign-btn {
            padding: 0.5rem 0.75rem;
            border: none;
            border-radius: 1rem;
            cursor: pointer;
            font-size: 0.85rem;
            font-weight: 500;
            transition: var(--transition);
        }

        .assign-btn.partner1 {
            background: var(--accent1);
            color: var(--bg-white);
        }

        .assign-btn.partner2 {
            background: var(--accent2);
            color: var(--bg-white);
        }

        .assign-btn.shared {
            background: var(--accent3);
            color: var(--bg-white);
        }

        .assign-btn.clear {
            background: #e5e7eb;
            color: var(--text-muted);
        }

        .summary {
            background: linear-gradient(145deg, #f8f9ff, #f0f4ff);
            border-radius: 0.75rem;
            padding: 1.5rem;
            margin-top: 2rem;
        }

        .summary-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
            gap: 1.5rem;
            margin-top: 1.5rem;
        }

        .summary-card {
            background: var(--bg-white);
            padding: 1.25rem;
            border-radius: 0.5rem;
            text-align: center;
            border: 2px solid transparent;
        }

        .summary-card.partner1 { border-color: #fda4af; } /* pinkish */
        .summary-card.partner2 { border-color: #93c5fd; } /* light blue */
        .summary-card.shared { border-color: #86efac; } /* green */

        .summary-card h4 {
            font-size: 1rem;
            margin-bottom: 0.75rem;
        }

        .summary-card .count {
            font-size: 1.75rem;
            font-weight: 700;
            color: var(--primary);
        }

        .ritual-section {
            background: linear-gradient(145deg, #fef2f2, #fee2e2);
            border-radius: 0.75rem;
            padding: 1.5rem;
            margin-top: 1.5rem;
        }

        .ritual-item, .tip-item {
            background: var(--bg-white);
            padding: 1rem;
            border-radius: 0.5rem;
            margin-bottom: 1rem;
            border-left: 4px solid var(--primary);
        }

        .add-task-btn {
            background: linear-gradient(135deg, var(--primary), var(--primary-dark));
            color: var(--bg-white);
            border: none;
            padding: 0.75rem 1.5rem;
            border-radius: 1.5rem;
            cursor: pointer;
            font-size: 1rem;
            font-weight: 600;
            margin-top: 1rem;
            transition: var(--transition);
        }

        .add-task-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(220, 38, 38, 0.2);
        }

        .custom-task-input {
            width: 100%;
            padding: 0.75rem;
            border: 1px solid #d1d5db;
            border-radius: 0.5rem;
            font-size: 1rem;
            margin-top: 0.75rem;
        }

        /* Mobile Sidebar */
        .sidebar-toggle {
            display: none;
            background: var(--primary);
            color: var(--bg-white);
            border: none;
            padding: 0.75rem;
            border-radius: 0.5rem;
            cursor: pointer;
            position: fixed;
            top: 1rem;
            right: 1rem;
            z-index: 1000;
        }

        @media (max-width: 768px) {
            .tabs {
                display: none;
                flex-direction: column;
                position: fixed;
                top: 0;
                left: 0;
                width: 250px;
                height: 100%;
                background: var(--bg-white);
                box-shadow: 2px 0 5px rgba(0, 0, 0, 0.1);
                transform: translateX(-100%);
                transition: transform 0.3s ease;
            }

            .tabs.active {
                display: flex;
                transform: translateX(0);
            }

            .tab {
                padding: 1.5rem;
                border-bottom: 1px solid #e5e7eb;
            }

            .sidebar-toggle {
                display: block;
            }

            .couple-setup, .tasks-grid, .summary-grid {
                grid-template-columns: 1fr;
            }

            .header h1 {
                font-size: 1.5rem;
            }
        }

        @media (max-width: 480px) {
            .container {
                border-radius: 0.5rem;
            }

            .content {
                padding: 1rem;
            }
        }
    </style>
<style type="text/css" id="operaUserStyle"></style></head>
<body>
    <button class="sidebar-toggle" aria-label="Toggle menu">☰</button>
    <div class="container">
        <div class="header">
            <h1>💕 Código Casa a Dois</h1>
            <p>Organizem suas tarefas sem brigas, cobranças ou sobrecarga</p>
        </div>

        <div class="tabs" role="tablist">
            <button class="tab active" role="tab" aria-selected="true" onclick="openTab(&#39;setup&#39;)">👫 Configuração</button>
            <button class="tab" role="tab" aria-selected="false" onclick="openTab(&#39;tasks&#39;)">📋 Tarefas</button>
            <button class="tab" role="tab" aria-selected="false" onclick="openTab(&#39;templates&#39;)">📊 Exemplos de Divisões</button>
            <button class="tab" role="tab" aria-selected="false" onclick="openTab(&#39;summary&#39;)">📈 Resumo</button>
            <button class="tab" role="tab" aria-selected="false" onclick="openTab(&#39;rituals&#39;)">💖 Rituais</button>
        </div>

        <div id="setup" class="content active" role="tabpanel">
            <h2 style="text-align: center; color: var(--primary); margin-bottom: 1.5rem;">Vamos Começar! Configurem o Casal</h2>
            <div class="couple-setup">
                <div class="partner">
                    <h3>👤 Parceiro(a) 1</h3>
                    <input type="text" id="partner1-name" placeholder="Nome do primeiro parceiro(a)" onchange="updatePartnerNames()">
                    <input type="text" id="partner1-energy" placeholder="Melhor horário de energia (ex: manhã)">
                    <input type="text" id="partner1-preferences" placeholder="Tarefas que gosta de fazer">
                    <input type="text" id="partner1-dislikes" placeholder="Tarefas que prefere evitar">
                </div>
                <div class="partner">
                    <h3>👤 Parceiro(a) 2</h3>
                    <input type="text" id="partner2-name" placeholder="Nome do segundo parceiro(a)" onchange="updatePartnerNames()">
                    <input type="text" id="partner2-energy" placeholder="Melhor horário de energia (ex: noite)">
                    <input type="text" id="partner2-preferences" placeholder="Tarefas que gosta de fazer">
                    <input type="text" id="partner2-dislikes" placeholder="Tarefas que prefere evitar">
                </div>
            </div>
            <div style="text-align: center; margin-top: 2rem;">
                <button class="add-task-btn" onclick="openTab(&#39;tasks&#39;)">Vamos Dividir as Tarefas! 🏠</button>
            </div>
        </div>

        <div id="tasks" class="content" role="tabpanel">
            <h2 style="text-align: center; color: var(--primary); margin-bottom: 1rem;">Divisão de Tarefas - Visual e Justa</h2>
            <p style="text-align: center; color: var(--text-muted); margin-bottom: 1.5rem;">
                Clique nos botões ao lado de cada tarefa para atribuí-la. 
                <span style="color: var(--accent1);">●</span> <span id="partner1-label">Parceiro 1</span> | 
                <span style="color: var(--accent2);">●</span> <span id="partner2-label">Parceiro 2</span> | 
                <span style="color: var(--accent3);">●</span> Compartilhada
            </p>
            <div class="tasks-grid" id="tasks-container"><div class="task-category"><h4>Cozinha</h4><div class="task-item" id="cozinha-0">
                        <span>Lavar louça</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;cozinha-0&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;cozinha-0&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;cozinha-0&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;cozinha-0&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="cozinha-1">
                        <span>Cozinhar almoço</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;cozinha-1&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;cozinha-1&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;cozinha-1&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;cozinha-1&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="cozinha-2">
                        <span>Cozinhar jantar</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;cozinha-2&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;cozinha-2&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;cozinha-2&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;cozinha-2&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="cozinha-3">
                        <span>Limpar fogão</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;cozinha-3&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;cozinha-3&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;cozinha-3&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;cozinha-3&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="cozinha-4">
                        <span>Limpar geladeira</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;cozinha-4&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;cozinha-4&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;cozinha-4&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;cozinha-4&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="cozinha-5">
                        <span>Organizar armários</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;cozinha-5&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;cozinha-5&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;cozinha-5&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;cozinha-5&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="cozinha-6">
                        <span>Fazer compras do supermercado</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;cozinha-6&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;cozinha-6&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;cozinha-6&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;cozinha-6&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="cozinha-7">
                        <span>Preparar lanche</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;cozinha-7&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;cozinha-7&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;cozinha-7&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;cozinha-7&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="cozinha-8">
                        <span>Lavar frutas e verduras</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;cozinha-8&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;cozinha-8&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;cozinha-8&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;cozinha-8&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><input type="text" class="custom-task-input" placeholder="Adicionar nova tarefa em Cozinha..."></div><div class="task-category"><h4>Limpeza</h4><div class="task-item" id="limpeza-0">
                        <span>Aspirar/varrer casa</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;limpeza-0&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;limpeza-0&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;limpeza-0&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;limpeza-0&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="limpeza-1">
                        <span>Passar pano no chão</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;limpeza-1&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;limpeza-1&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;limpeza-1&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;limpeza-1&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="limpeza-2">
                        <span>Limpar banheiros</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;limpeza-2&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;limpeza-2&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;limpeza-2&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;limpeza-2&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="limpeza-3">
                        <span>Tirar pó dos móveis</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;limpeza-3&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;limpeza-3&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;limpeza-3&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;limpeza-3&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="limpeza-4">
                        <span>Limpar espelhos</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;limpeza-4&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;limpeza-4&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;limpeza-4&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;limpeza-4&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="limpeza-5">
                        <span>Organizar quartos</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;limpeza-5&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;limpeza-5&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;limpeza-5&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;limpeza-5&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="limpeza-6">
                        <span>Limpar janelas</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;limpeza-6&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;limpeza-6&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;limpeza-6&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;limpeza-6&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="limpeza-7">
                        <span>Aspirar sofá e tapetes</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;limpeza-7&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;limpeza-7&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;limpeza-7&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;limpeza-7&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><input type="text" class="custom-task-input" placeholder="Adicionar nova tarefa em Limpeza..."></div><div class="task-category"><h4>Roupas</h4><div class="task-item" id="roupas-0">
                        <span>Lavar roupa</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;roupas-0&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;roupas-0&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;roupas-0&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;roupas-0&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="roupas-1">
                        <span>Estender roupa</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;roupas-1&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;roupas-1&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;roupas-1&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;roupas-1&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="roupas-2">
                        <span>Recolher roupa seca</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;roupas-2&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;roupas-2&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;roupas-2&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;roupas-2&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="roupas-3">
                        <span>Dobrar e guardar roupas</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;roupas-3&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;roupas-3&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;roupas-3&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;roupas-3&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="roupas-4">
                        <span>Passar roupa</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;roupas-4&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;roupas-4&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;roupas-4&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;roupas-4&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="roupas-5">
                        <span>Organizar guarda-roupa</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;roupas-5&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;roupas-5&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;roupas-5&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;roupas-5&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="roupas-6">
                        <span>Separar roupas para lavar</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;roupas-6&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;roupas-6&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;roupas-6&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;roupas-6&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="roupas-7">
                        <span>Lavar tênis e sapatos</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;roupas-7&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;roupas-7&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;roupas-7&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;roupas-7&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><input type="text" class="custom-task-input" placeholder="Adicionar nova tarefa em Roupas..."></div><div class="task-category"><h4>Área Externa</h4><div class="task-item" id="área externa-0">
                        <span>Regar plantas</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;área externa-0&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;área externa-0&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;área externa-0&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;área externa-0&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="área externa-1">
                        <span>Cuidar do jardim</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;área externa-1&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;área externa-1&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;área externa-1&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;área externa-1&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="área externa-2">
                        <span>Limpar quintal/varanda</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;área externa-2&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;área externa-2&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;área externa-2&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;área externa-2&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="área externa-3">
                        <span>Recolher lixo</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;área externa-3&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;área externa-3&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;área externa-3&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;área externa-3&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="área externa-4">
                        <span>Cuidar de pets</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;área externa-4&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;área externa-4&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;área externa-4&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;área externa-4&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="área externa-5">
                        <span>Lavar carro</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;área externa-5&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;área externa-5&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;área externa-5&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;área externa-5&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="área externa-6">
                        <span>Organizar área de serviço</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;área externa-6&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;área externa-6&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;área externa-6&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;área externa-6&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="área externa-7">
                        <span>Manutenção de equipamentos</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;área externa-7&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;área externa-7&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;área externa-7&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;área externa-7&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><input type="text" class="custom-task-input" placeholder="Adicionar nova tarefa em Área Externa..."></div><div class="task-category"><h4>Administração</h4><div class="task-item" id="administração-0">
                        <span>Pagar contas</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;administração-0&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;administração-0&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;administração-0&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;administração-0&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="administração-1">
                        <span>Organizar documentos</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;administração-1&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;administração-1&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;administração-1&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;administração-1&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="administração-2">
                        <span>Controlar orçamento</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;administração-2&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;administração-2&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;administração-2&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;administração-2&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="administração-3">
                        <span>Agendar consultas médicas</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;administração-3&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;administração-3&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;administração-3&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;administração-3&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="administração-4">
                        <span>Resolver questões burocráticas</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;administração-4&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;administração-4&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;administração-4&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;administração-4&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="administração-5">
                        <span>Planejamento de viagens</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;administração-5&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;administração-5&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;administração-5&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;administração-5&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><div class="task-item" id="administração-6">
                        <span>Controle de investimentos</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask(&#39;administração-6&#39;, &#39;partner1&#39;)">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask(&#39;administração-6&#39;, &#39;partner2&#39;)">P2</button>
                            <button class="assign-btn shared" onclick="assignTask(&#39;administração-6&#39;, &#39;shared&#39;)">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask(&#39;administração-6&#39;, &#39;none&#39;)">×</button>
                        </div>
                    </div><input type="text" class="custom-task-input" placeholder="Adicionar nova tarefa em Administração..."></div></div>
        </div>

        <div id="templates" class="content" role="tabpanel">
            <div style="background: linear-gradient(145deg, #fef2f2, #fee2e2); border-radius: 0.75rem; padding: 1.5rem;">
                <h2 style="text-align: center; color: var(--primary); margin-bottom: 1rem;">📊 Exemplos de Divisões para Diferentes Rotinas</h2>
                <p style="text-align: center; color: var(--text-muted); margin-bottom: 1.5rem;">
                    Planilhas e quadros visuais adaptáveis ao estilo de vida do casal.
                </p>
                <div class="tasks-grid">
                    <div class="task-category">
                        <h4>👨‍💼 Modelo: Ambos Trabalham Fora</h4>
                        <p style="font-size: 0.9rem; margin-bottom: 1rem; color: var(--text-muted);">Para casais com rotinas de trabalho externas.</p>
                        <div class="task-item assigned-partner1">
                            <span>Manhãs: Café da manhã + arrumar cama</span>
                        </div>
                        <div class="task-item assigned-partner2">
                            <span>Noites: Jantar + organizar cozinha</span>
                        </div>
                        <div class="task-item shared">
                            <span>Fins de semana: Limpeza geral juntos</span>
                        </div>
                        <div class="task-item assigned-partner1">
                            <span>Roupas: Lavar e estender</span>
                        </div>
                        <div class="task-item assigned-partner2">
                            <span>Compras: Supermercado</span>
                        </div>
                        <button class="add-task-btn" onclick="applyTemplate(&#39;ambos_fora&#39;)">Aplicar Este Modelo</button>
                    </div>

                    <div class="task-category">
                        <h4>🏠 Modelo: Um Trabalha Fora, Outro em Casa</h4>
                        <p style="font-size: 0.9rem; margin-bottom: 1rem; color: var(--text-muted);">Para casais onde um trabalha em casa.</p>
                        <div class="task-item assigned-partner1">
                            <span>Em casa: Limpeza diária + almoço</span>
                        </div>
                        <div class="task-item assigned-partner2">
                            <span>Trabalha fora: Jantar + feira</span>
                        </div>
                        <div class="task-item shared">
                            <span>Roupas: Divisão meio a meio</span>
                        </div>
                        <div class="task-item assigned-partner1">
                            <span>Em casa: Receber entregas + pets</span>
                        </div>
                        <div class="task-item assigned-partner2">
                            <span>Trabalha fora: Contas + burocracias</span>
                        </div>
                        <button class="add-task-btn" onclick="applyTemplate(&#39;um_fora&#39;)">Aplicar Este Modelo</button>
                    </div>

                    <div class="task-category">
                        <h4>🏡 Modelo: Ambos em Casa</h4>
                        <p style="font-size: 0.9rem; margin-bottom: 1rem; color: var(--text-muted);">Para casais que ficam em casa.</p>
                        <div class="task-item shared">
                            <span>Cozinha: Alternar refeições</span>
                        </div>
                        <div class="task-item assigned-partner1">
                            <span>Limpeza: Áreas comuns</span>
                        </div>
                        <div class="task-item assigned-partner2">
                            <span>Roupas: Lavar e organizar</span>
                        </div>
                        <div class="task-item shared">
                            <span>Compras: Planejar juntos</span>
                        </div>
                        <div class="task-item assigned-partner1">
                            <span>Administração: Contas</span>
                        </div>
                        <button class="add-task-btn" onclick="applyTemplate(&#39;ambos_casa&#39;)">Aplicar Este Modelo</button>
                    </div>
                </div>

                <div style="background: white; border-radius: 10px; padding: 20px; margin-top: 30px; border-left: 5px solid var(--primary);">
                    <h4 style="color: var(--primary); margin-bottom: 15px;">💡 Como Personalizar Qualquer Modelo:</h4>
                    <p style="margin-bottom: 10px;"><strong>1. Considerem horários:</strong> Quem está disponível quando?</p>
                    <p style="margin-bottom: 10px;"><strong>2. Identifiquem preferências:</strong> O que cada um gosta/odeia fazer?</p>
                    <p style="margin-bottom: 10px;"><strong>3. Equilibrem energia:</strong> Tarefas pesadas + tarefas leves</p>
                    <p style="margin-bottom: 10px;"><strong>4. Testem por 2 semanas</strong> antes de ajustar</p>
                    <p><strong>5. Sejam flexíveis:</strong> A vida muda, a divisão também pode mudar!</p>
                </div>
            </div>
        </div>

        <div id="summary" class="content" role="tabpanel">
            <div class="summary">
                <h2 style="text-align: center; color: var(--primary); margin-bottom: 1rem;">📊 Resumo da Divisão</h2>
                <div class="summary-grid">
                    <div class="summary-card partner1">
                        <h4 id="summary-partner1">Parceiro 1</h4>
                        <div class="count" id="count-partner1">0</div>
                        <p>tarefas atribuídas</p>
                    </div>
                    <div class="summary-card partner2">
                        <h4 id="summary-partner2">Parceiro 2</h4>
                        <div class="count" id="count-partner2">0</div>
                        <p>tarefas atribuídas</p>
                    </div>
                    <div class="summary-card shared">
                        <h4>Compartilhadas</h4>
                        <div class="count" id="count-shared">0</div>
                        <p>tarefas em dupla</p>
                    </div>
                </div>
                <div id="balance-feedback" style="text-align: center; margin-top: 1.5rem; padding: 1rem; border-radius: 0.5rem; background: rgb(245, 245, 245);"><p style="color: var(--text-muted);">Comece atribuindo algumas tarefas para ver o equilíbrio!</p></div>
            </div>
        </div>

        <div id="rituals" class="content" role="tabpanel">
            <div class="ritual-section">
                <h2 style="text-align: center; color: var(--primary); margin-bottom: 1rem;">💖 Guia de Rituais para Fortalecer o Vínculo</h2>
                <p style="text-align: center; color: var(--text-muted); margin-bottom: 1.5rem;">
                    Rituais rápidos e significativos para fortalecer o vínculo do casal enquanto alinham tarefas e expectativas.
                </p>

                <div class="ritual-item">
                    <h4>📅 Ritual 1: Reunião Semanal de Alinhamento (15 min)</h4>
                    <p><strong>Quando:</strong> Toda sexta à noite<br>
                    <strong>Como:</strong> Sentem juntos e revisem: "O que funcionou bem esta semana? O que pode melhorar?" Ajustem as tarefas da próxima semana se necessário.</p>
                </div>

                <div class="ritual-item">
                    <h4>☕ Ritual 2: Café da Conexão e Planejamento</h4>
                    <p><strong>Quando:</strong> Domingos de manhã<br>
                    <strong>Como:</strong> Preparem café/chá juntos. Conversem sobre a semana que vem, mas também sobre sonhos e planos. Misturem organização com intimidade.</p>
                </div>

                <div class="ritual-item">
                    <h4>🎉 Ritual 3: Celebração das Conquistas</h4>
                    <p><strong>Quando:</strong> Quando completarem 100% das tarefas semanais<br>
                    <strong>Como:</strong> Pizza, filme, jantar especial ou qualquer coisa que gostem de fazer juntos. Celebrem a parceria!</p>
                </div>

                <div class="ritual-item">
                    <h4>🔄 Ritual 4: Check-in Carinhoso de Meio de Semana</h4>
                    <p><strong>Quando:</strong> Quarta-feira<br>
                    <strong>Como:</strong> 5 minutos apenas: "Como está sendo pra você?" Escutem sem julgar e façam pequenos ajustes se necessário.</p>
                </div>

                <div class="ritual-item">
                    <h4>💌 Ritual 5: Cartão de Gratidão Surpresa</h4>
                    <p><strong>Quando:</strong> Quando o parceiro faz algo além do combinado<br>
                    <strong>Como:</strong> Deixem bilhetinhos de agradecimento. "Obrigado(a) por ter feito X sem eu pedir. Isso me fez sentir..."</p>
                </div>

                <div class="ritual-item">
                    <h4>🌅 Ritual 6: Manhã da Parceria</h4>
                    <p><strong>Como:</strong> Uma vez por mês, acordem 30 min mais cedo e façam as tarefas matinais juntos ouvindo música. Transformem obrigação em diversão.</p>
                </div>

                <div class="ritual-item">
                    <h4>🎯 Ritual 7: Desafio Semanal da Casa</h4>
                    <p><strong>Como:</strong> Escolham uma área da casa para melhorar juntos na semana. Ex: "Semana da cozinha organizada". Trabalhem como equipe.</p>
                </div>

                <div class="ritual-item">
                    <h4>💝 Ritual 8: Troca de Tarefas Surpresa</h4>
                    <p><strong>Como:</strong> Uma vez por mês, cada um faz uma tarefa que normalmente é do outro. Demonstrem cuidado e quebrem a rotina.</p>
                </div>

                <div class="ritual-item">
                    <h4>📱 Ritual 9: Foto da Conquista</h4>
                    <p><strong>Como:</strong> Tirem uma foto juntos quando terminarem uma grande limpeza ou organização. Criem memórias positivas das tarefas de casa.</p>
                </div>

                <div class="ritual-item">
                    <h4>🍷 Ritual 10: Conversa da Evolução</h4>
                    <p><strong>Quando:</strong> Fim do mês<br>
                    <strong>Como:</strong> Com uma bebida que gostem, conversem: "Como nossa parceria evoluiu?" Celebrem o crescimento como casal.</p>
                </div>

                <div style="background: white; border-radius: 10px; padding: 20px; margin-top: 30px; border-left: 5px solid var(--primary);">
                    <h4 style="color: var(--primary); margin-bottom: 15px;">💡 Dicas para Fortalecer a Relação:</h4>
                    <p style="margin-bottom: 10px;"><strong>1. Comunicação Aberta:</strong> Expressem gratidão diariamente pelas pequenas coisas.</p>
                    <p style="margin-bottom: 10px;"><strong>2. Tempo de Qualidade:</strong> Dediquem momentos exclusivos para o casal, sem distrações.</p>
                    <p style="margin-bottom: 10px;"><strong>3. Apoio Mútuo:</strong> Apoiem os sonhos e objetivos um do outro.</p>
                    <p style="margin-bottom: 10px;"><strong>4. Surpresas:</strong> Façam gestos inesperados de carinho.</p>
                    <p><strong>5. Resolução de Conflitos:</strong> Foquem em soluções juntos, não em culpas.</p>
                </div>
            </div>
        </div>
    </div>

    <script>
        const taskCategories = {
            'Cozinha': [
                'Lavar louça', 'Cozinhar almoço', 'Cozinhar jantar', 'Limpar fogão', 
                'Limpar geladeira', 'Organizar armários', 'Fazer compras do supermercado',
                'Preparar lanche', 'Lavar frutas e verduras'
            ],
            'Limpeza': [
                'Aspirar/varrer casa', 'Passar pano no chão', 'Limpar banheiros', 
                'Tirar pó dos móveis', 'Limpar espelhos', 'Organizar quartos',
                'Limpar janelas', 'Aspirar sofá e tapetes'
            ],
            'Roupas': [
                'Lavar roupa', 'Estender roupa', 'Recolher roupa seca', 
                'Dobrar e guardar roupas', 'Passar roupa', 'Organizar guarda-roupa',
                'Separar roupas para lavar', 'Lavar tênis e sapatos'
            ],
            'Área Externa': [
                'Regar plantas', 'Cuidar do jardim', 'Limpar quintal/varanda', 
                'Recolher lixo', 'Cuidar de pets', 'Lavar carro',
                'Organizar área de serviço', 'Manutenção de equipamentos'
            ],
            'Administração': [
                'Pagar contas', 'Organizar documentos', 'Controlar orçamento', 
                'Agendar consultas médicas', 'Resolver questões burocráticas',
                'Planejamento de viagens', 'Controle de investimentos'
            ]
        };

        let taskAssignments = {};

        const templateModels = {
            ambos_fora: {
                'cozinha-0': 'partner1',    // Lavar louça
                'cozinha-1': 'partner1',    // Cozinhar almoço
                'cozinha-2': 'partner2',    // Cozinhar jantar
                'cozinha-6': 'partner2',    // Fazer compras
                'limpeza-0': 'shared',      // Aspirar/varrer
                'limpeza-2': 'shared',      // Limpar banheiros
                'roupas-0': 'partner1',     // Lavar roupa
                'roupas-3': 'partner2',     // Dobrar roupas
            },
            um_fora: {
                'cozinha-0': 'partner1',    // Lavar louça (em casa)
                'cozinha-1': 'partner1',    // Almoço
                'cozinha-2': 'partner2',    // Jantar (fora)
                'cozinha-6': 'partner2',    // Compras
                'limpeza-0': 'partner1',    // Limpeza diária
                'roupas-0': 'shared',       // Roupas divididas
                'administração-0': 'partner2' // Contas
            },
            ambos_casa: {
                'cozinha-1': 'shared',      // Alternar refeições
                'limpeza-0': 'partner1',    // Áreas comuns
                'roupas-0': 'partner2',     // Lavar e organizar
                'cozinha-6': 'shared',      // Compras juntos
                'administração-0': 'partner1' // Contas
            }
        };

        function applyTemplate(templateName) {
            const template = templateModels[templateName];
            if (!template) return;

            Object.keys(taskAssignments).forEach(taskId => {
                const taskElement = document.getElementById(taskId);
                if (taskElement) {
                    taskElement.classList.remove('assigned-partner1', 'assigned-partner2', 'shared');
                }
            });
            taskAssignments = {};

            Object.entries(template).forEach(([taskId, assignment]) => {
                const taskElement = document.getElementById(taskId);
                if (taskElement) {
                    taskElement.classList.add(`assigned-${assignment}`);
                    taskAssignments[taskId] = assignment;
                }
            });

            updateSummary();
            alert(`Modelo "${templateName}" aplicado! Vá para a aba Resumo para ver o resultado.`);
            openTab('summary');
        }

        function openTab(tabName) {
            document.querySelectorAll('.content').forEach(content => content.classList.remove('active'));
            document.querySelectorAll('.tab').forEach(tab => {
                tab.classList.remove('active');
                tab.setAttribute('aria-selected', 'false');
            });

            const selectedContent = document.getElementById(tabName);
            selectedContent.classList.add('active');
            const selectedTab = document.querySelector(`.tab[onclick="openTab('${tabName}')"]`);
            selectedTab.classList.add('active');
            selectedTab.setAttribute('aria-selected', 'true');

            document.querySelector('.tabs').classList.remove('active');
        }

        function updatePartnerNames() {
            const partner1Name = document.getElementById('partner1-name').value || 'Parceiro 1';
            const partner2Name = document.getElementById('partner2-name').value || 'Parceiro 2';

            document.getElementById('partner1-label').textContent = partner1Name;
            document.getElementById('partner2-label').textContent = partner2Name;
            document.getElementById('summary-partner1').textContent = partner1Name;
            document.getElementById('summary-partner2').textContent = partner2Name;

            updateTaskButtons();
        }

        function updateTaskButtons() {
            const partner1Name = document.getElementById('partner1-name').value || 'P1';
            const partner2Name = document.getElementById('partner2-name').value || 'P2';
            
            document.querySelectorAll('.assign-btn.partner1').forEach(btn => btn.textContent = partner1Name.substring(0, 3));
            document.querySelectorAll('.assign-btn.partner2').forEach(btn => btn.textContent = partner2Name.substring(0, 3));
        }

        function createTasksGrid() {
            const container = document.getElementById('tasks-container');
            
            Object.keys(taskCategories).forEach(category => {
                const categoryDiv = document.createElement('div');
                categoryDiv.className = 'task-category';
                
                const categoryTitle = document.createElement('h4');
                categoryTitle.textContent = category;
                categoryDiv.appendChild(categoryTitle);
                
                taskCategories[category].forEach((task, index) => {
                    const taskId = `${category.toLowerCase()}-${index}`;
                    const taskItem = document.createElement('div');
                    taskItem.className = 'task-item';
                    taskItem.id = taskId;
                    
                    taskItem.innerHTML = `
                        <span>${task}</span>
                        <div class="task-controls">
                            <button class="assign-btn partner1" onclick="assignTask('${taskId}', 'partner1')">P1</button>
                            <button class="assign-btn partner2" onclick="assignTask('${taskId}', 'partner2')">P2</button>
                            <button class="assign-btn shared" onclick="assignTask('${taskId}', 'shared')">Ambos</button>
                            <button class="assign-btn clear" onclick="assignTask('${taskId}', 'none')">×</button>
                        </div>
                    `;
                    
                    categoryDiv.appendChild(taskItem);
                });

                const customInput = document.createElement('input');
                customInput.type = 'text';
                customInput.className = 'custom-task-input';
                customInput.placeholder = `Adicionar nova tarefa em ${category}...`;
                customInput.addEventListener('keypress', function(e) {
                    if (e.key === 'Enter' && this.value.trim()) {
                        addCustomTask(category, this.value.trim());
                        this.value = '';
                    }
                });
                
                categoryDiv.appendChild(customInput);
                container.appendChild(categoryDiv);
            });
        }

        function addCustomTask(category, taskName) {
            const categoryDiv = document.querySelector(`.task-category h4:contains('${category}')`).parentElement;
            const customInput = categoryDiv.querySelector('.custom-task-input');
            
            const taskId = `${category.toLowerCase()}-custom-${Date.now()}`;
            const taskItem = document.createElement('div');
            taskItem.className = 'task-item';
            taskItem.id = taskId;
            
            taskItem.innerHTML = `
                <span>${taskName}</span>
                <div class="task-controls">
                    <button class="assign-btn partner1" onclick="assignTask('${taskId}', 'partner1')">P1</button>
                    <button class="assign-btn partner2" onclick="assignTask('${taskId}', 'partner2')">P2</button>
                    <button class="assign-btn shared" onclick="assignTask('${taskId}', 'shared')">Ambos</button>
                    <button class="assign-btn clear" onclick="assignTask('${taskId}', 'none')">×</button>
                </div>
            `;
            
            categoryDiv.insertBefore(taskItem, customInput);
            updateTaskButtons();
        }

        function assignTask(taskId, assignment) {
            const taskElement = document.getElementById(taskId);
            taskElement.classList.remove('assigned-partner1', 'assigned-partner2', 'shared');
            
            if (assignment !== 'none') {
                taskElement.classList.add(`assigned-${assignment}`);
                taskAssignments[taskId] = assignment;
            } else {
                delete taskAssignments[taskId];
            }
            
            updateSummary();
        }

        function updateSummary() {
            let counts = { partner1: 0, partner2: 0, shared: 0 };

            Object.values(taskAssignments).forEach(assignment => {
                if (counts[assignment] !== undefined) counts[assignment]++;
            });

            document.getElementById('count-partner1').textContent = counts.partner1;
            document.getElementById('count-partner2').textContent = counts.partner2;
            document.getElementById('count-shared').textContent = counts.shared;

            const total = counts.partner1 + counts.partner2;
            const feedbackDiv = document.getElementById('balance-feedback');
            
            if (total === 0) {
                feedbackDiv.innerHTML = '<p style="color: var(--text-muted);">Comece atribuindo algumas tarefas para ver o equilíbrio!</p>';
                feedbackDiv.style.background = '#f5f5f5';
            } else {
                const difference = Math.abs(counts.partner1 - counts.partner2);
                
                if (difference <= 1) {
                    feedbackDiv.innerHTML = '<p style="color: #16a34a;">🎉 Divisão equilibrada! Vocês estão no caminho certo!</p>';
                    feedbackDiv.style.background = '#dcfce7';
                } else if (difference <= 3) {
                    feedbackDiv.innerHTML = '<p style="color: #d97706;">⚖️ Quase lá! Pequeno ajuste pode melhorar o equilíbrio.</p>';
                    feedbackDiv.style.background = '#fefcbf';
                } else {
                    feedbackDiv.innerHTML = '<p style="color: var(--primary);">🔄 Que tal redistribuir algumas tarefas para equilibrar melhor?</p>';
                    feedbackDiv.style.background = '#fee2e2';
                }
            }
        }

        document.querySelector('.sidebar-toggle').addEventListener('click', () => {
            document.querySelector('.tabs').classList.toggle('active');
        });

        document.addEventListener('DOMContentLoaded', () => {
            createTasksGrid();
            updateSummary();
        });
    </script>

</body></html>
