<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Calculadora de Porcentagem</title>
<style>
    body {
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        background-color: #121212;
        color: #ffffff;
        margin: 0;
        padding: 15px;
    }
    h2 { 
        text-align: center; 
        margin-bottom: 15px; 
        color: #00e0ff; 
        font-size: 1.4rem;
    }
    .container { 
        display: flex; 
        flex-direction: column; 
        gap: 15px; 
        max-width: 100%; 
        margin: auto; 
    }
    .section { 
        background: #1a1a1a; 
        padding: 12px; 
        border-radius: 10px; 
        box-shadow: 0 0 10px rgba(0,0,0,0.4); 
    }
    .section h3 { 
        margin-bottom: 10px; 
        color: #00b8cc; 
        font-size: 1rem;
        display: flex; 
        align-items: center; 
        gap: 5px; 
    }
    .inputs { 
        display: flex; 
        flex-direction: column; 
        gap: 12px; 
    }
    label { 
        display: flex; 
        flex-direction: column; 
        font-size: 14px; 
    }
    input { 
        padding: 10px; 
        border-radius: 6px; 
        border: none; 
        width: 100%; 
        background-color: #1e1e1e; 
        color: #ffffff; 
        text-align: center; 
        font-size: 1rem;
    }
    button { 
        padding: 12px; 
        border-radius: 6px; 
        border: none; 
        background-color: #00e0ff; 
        color: #121212; 
        font-weight: bold; 
        cursor: pointer; 
        transition: background-color 0.3s; 
        width: 100%;
        margin-top: 10px;
        font-size: 1rem;
    }
    button:hover { background-color: #00b8cc; }

    /* Tooltip */
    .tooltip { display: inline-block; cursor: pointer; position: relative; }
    .tooltip .tooltiptext {
        visibility: hidden; width: 200px; background-color: #333; color: #fff; text-align: center;
        border-radius: 8px; padding: 8px; position: absolute; z-index: 1; bottom: 125%; left: 50%; margin-left: -100px;
        opacity: 0; transition: opacity 0.3s; font-size: 12px;
    }
    .tooltip:hover .tooltiptext { visibility: visible; opacity: 1; }

    /* Resultados em coluna no celular */
    .resultado-grid {
        display: flex;
        flex-direction: column;
        gap: 10px;
        margin-top: 15px;
    }
    .resultado-card {
        background: #1f1f1f;
        padding: 12px;
        border-radius: 8px;
        text-align: center;
        box-shadow: 0 0 5px rgba(0,0,0,0.3);
        font-size: 0.9rem;
    }
    .resultado-card strong { display: block; color: #00e0ff; margin-bottom: 4px; font-size: 0.95rem; }

    /* Ajustes para telas maiores */
    @media (min-width: 600px) {
        .inputs { 
            flex-direction: row; 
            flex-wrap: wrap; 
            justify-content: center; 
        }
        input { width: 120px; }
        .resultado-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
        }
    }
</style>
</head>
<body>

<h2>Calculadora de Porcentagem</h2>
<div class="container">

    <!-- Horário principal -->
    <div class="section">
        <h3>Horário de Trabalho 
            <span class="tooltip">ℹ️
                <span class="tooltiptext">Informe a hora de entrada e saída do trabalho.</span>
            </span>
        </h3>
        <div class="inputs">
            <label>Entrada:
                <input type="text" id="horaEntrada" placeholder="7:30" maxlength="5" oninput="formatarHora(this)">
            </label>
            <label>Saída:
                <input type="text" id="horaSaida" placeholder="17:30" maxlength="5" oninput="formatarHora(this)">
            </label>
        </div>
    </div>

    <!-- Horário parado -->
    <div class="section">
        <h3>Tempo Parado (opcional)
            <span class="tooltip">ℹ️
                <span class="tooltiptext">Em horas: informe início e fim. Em minutos: informe apenas o tempo.</span>
            </span>
        </h3>
        <div class="inputs">
            <label>Início/Fim ou Tempo:
                <input type="text" id="horaParadoInicio" placeholder="9:00" maxlength="5" oninput="formatarHora(this)">
                <input type="text" id="horaParadoFim" placeholder="9:30" maxlength="5" oninput="formatarHora(this)">
            </label>
            <label style="flex-direction:row; align-items:center; gap:8px;">
                <input type="checkbox" id="paradoEmHoras">
                <span>Usar horas</span>
            </label>
        </div>
    </div>

    <!-- Inputs principais -->
    <div class="section">
        <h3>Parâmetros</h3>
        <div class="inputs">
            <label>pc/h: <input type="text" id="pc_h"></label>
            <label>nC:
                <input type="text" id="nC">
                <span class="tooltip">ℹ️
                    <span class="tooltiptext">Número de colaboradores.</span>
                </span>
            </label>
            <label>Lb: <input type="text" id="Lb"></label>
            <label>Pro: <input type="text" id="Pro"></label>
        </div>
        <button onclick="calcular()">Calcular</button>
    </div>

    <!-- Resultados -->
    <div class="section">
        <h3>Resultados</h3>
        <div id="resultadoContainer" class="resultado-grid"></div>
    </div>

</div>

<div style="text-align:center; margin-top:15px; font-size:12px; color:#888;">
    Criado e desenvolvido por Rafael Szekut
</div>

<script>
/* Mesmo JS do seu código original */
function formatarHora(input) {
    let v = input.value.replace(/\D/g,'');
    if(v.length <= 2){ input.value = parseInt(v,10)||''; }
    else { let h=parseInt(v.slice(0,-2),10); let m=v.slice(-2); if(h||m){ input.value=h+':'+m; } }
}
function parseVirgula(valor){ return parseFloat(valor.replace(',','.')); }
function diferencaEmMinutos(h1,h2){ if(!h1||!h2) return 0; const [a,b]=h1.split(':').map(Number); const [c,d]=h2.split(':').map(Number); if(isNaN(a)||isNaN(b)||isNaN(c)||isNaN(d)) return 0; return (c*60+d)-(a*60+b); }
function calcularMinutos(){
    const entrada=document.getElementById('horaEntrada').value;
    const saida=document.getElementById('horaSaida').value;
    const inicioParado=document.getElementById('horaParadoInicio').value;
    const fimParado=document.getElementById('horaParadoFim').value;
    const paradoEmHoras=document.getElementById('paradoEmHoras').checked;
    if(!entrada||!saida) return null;
    const [hEntrada,mEntrada]=entrada.split(':').map(Number);
    const [hSaida,mSaida]=saida.split(':').map(Number);
    if(isNaN(hEntrada)||isNaN(mEntrada)||isNaN(hSaida)||isNaN(mSaida)) return null;
    let tt=(hSaida*60+mSaida)-(hEntrada*60+mEntrada);
    const inicioCritico=8*60+50, fimCritico=9*60+5;
    const inicioT=hEntrada*60+mEntrada, fimT=hSaida*60+mSaida;
    let MD=0;
    if(fimT>inicioCritico && inicioT<fimCritico){ tt-=15; MD+=15; }
    let tempoParado=0;
    if(inicioParado){
        if(paradoEmHoras){
            if(fimParado){
                tempoParado=diferencaEmMinutos(inicioParado,fimParado);
            }
        } else {
            tempoParado=parseFloat(inicioParado);
        }
        if(tempoParado>0){ tt-=tempoParado; MD+=tempoParado; }
    }
    return {tt:tt<0?0:tt, MD};
}
function calcular(){
    const pc_h=parseVirgula(document.getElementById('pc_h').value);
    const nC=parseVirgula(document.getElementById('nC').value);
    const Lb=parseVirgula(document.getElementById('Lb').value);
    const Pro=parseVirgula(document.getElementById('Pro').value);
    const minutosData=calcularMinutos();
    if(minutosData===null){ alert("Digite horários válidos."); return; }
    const tt=minutosData.tt, MD=minutosData.MD;
    if(isNaN(pc_h)||isNaN(nC)||isNaN(Lb)||isNaN(Pro)||Lb===0){ alert("Preencha todos os parâmetros corretamente (Lb não pode ser zero)."); return; }
    const Req=(pc_h*nC*tt)/(Lb*60);
    const ResultadoFinal=(Pro*100)/Req;
    const container=document.getElementById('resultadoContainer');
    container.innerHTML=`
        <div class="resultado-card"><strong>pc/h</strong>${pc_h}</div>
        <div class="resultado-card"><strong>nC</strong>${nC}</div>
        <div class="resultado-card"><strong>tt (min)</strong>${tt}</div>
        <div class="resultado-card"><strong>Lb</strong>${Lb}</div>
        <div class="resultado-card"><strong>Req</strong>${Req.toFixed(2)}</div>
        <div class="resultado-card"><strong>Pro</strong>${Pro}</div>
        <div class="resultado-card"><strong>Resultado Final (%)</strong>${ResultadoFinal.toFixed(1)}</div>
        <div class="resultado-card"><strong>MD (Minutos Descontados)
            <span class="tooltip">ℹ️
                <span class="tooltiptext">Tempo descontado do total por parada ou intervalo crítico.</span>
            </span>
        </strong>${MD}</div>
    `;
}
</script>

</body>
</html>
