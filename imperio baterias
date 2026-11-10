import React, { useState, useEffect } from 'react';
import { 
  Package, ShoppingCart, DollarSign, RefreshCw, FileText, 
  Plus, Trash2, CheckCircle, AlertTriangle, Calendar, Search, 
  TrendingUp, Scale, Mail, Check, Menu, ArrowLeft, BatteryCharging
} from 'lucide-react';

export default function ImperioBateriasApp() {
  // --- ESTADOS PERSISTIDOS NO LOCALSTORAGE ---
  const [baterias, setBaterias] = useState(() => {
    const saved = localStorage.getItem('imp_baterias');
    return saved ? JSON.parse(saved) : [
      { id: 1, modelo: "Automotiva Convencional", marca: "Moura", amperagem: "60 Ah", estoque: 12, minEstoque: 3, precoVenda: 450, custo: 320 },
      { id: 2, modelo: "Automotiva Selada", marca: "Heliar", amperagem: "70 Ah", estoque: 8, minEstoque: 2, precoVenda: 580, custo: 410 },
      { id: 3, modelo: "Pesada / Caminhão", marca: "Moura", amperagem: "150 Ah", estoque: 4, minEstoque: 1, precoVenda: 1150, custo: 850 },
      { id: 4, modelo: "Pesada / Caminhão", marca: "Bosch", amperagem: "100 Ah", estoque: 5, minEstoque: 2, precoVenda: 820, custo: 600 }
    ];
  });

  const [sucatas, setSucatas] = useState(() => {
    const saved = localStorage.getItem('imp_sucatas');
    return saved ? JSON.parse(saved) : [
      { id: 1, descricao: "Sucata 60Ah (Moura/Heliar)", pesoKg: 14.5, quantidade: 1, data: "2026-11-10" },
      { id: 2, descricao: "Sucata 150Ah (Caminhão)", pesoKg: 35.0, quantidade: 1, data: "2026-11-10" }
    ];
  });

  const hojeStr = new Date().toLocaleDateString('pt-BR');

  const [vendas, setVendas] = useState(() => {
    const saved = localStorage.getItem('imp_vendas');
    return saved ? JSON.parse(saved) : [
      { id: 101, data: `${hojeStr} 10:30`, dataCurta: hojeStr, cliente: "João da Silva", item: "Moura 60 Ah", qtd: 1, valorUnit: 450, total: 450, tipoPagto: "Pix", sucataGerada: "1x 60Ah" },
      { id: 102, data: `${hojeStr} 14:15`, dataCurta: hojeStr, cliente: "Transportadora Rota Sul", item: "Moura 150 Ah", qtd: 2, valorUnit: 1150, total: 2300, tipoPagto: "Boleto", sucataGerada: "2x 150Ah" }
    ];
  });

  const [dividas, setDividas] = useState(() => {
    const saved = localStorage.getItem('imp_dividas');
    return saved ? JSON.parse(saved) : [
      { id: 1, tipo: "receber", nome: "Carlos Oficina", valor: 450, vencimento: "2026-11-15", status: "PENDENTE" },
      { id: 2, tipo: "pagar", nome: "Distribuidora Moura RS", valor: 3200, vencimento: "2026-11-12", status: "PENDENTE" }
    ];
  });

  // 'inicio' é a tela principal tipo maquininha. 'menu' exibe os módulos.
  const [abaAtiva, setAbaAtiva] = useState('inicio');
  const [mensagemSucesso, setMensagemSucesso] = useState('');

  // Estados de Nova Venda
  const [vendaBateriaId, setVendaBateriaId] = useState('');
  const [vendaQtd, setVendaQtd] = useState(1);
  const [vendaCliente, setVendaCliente] = useState('');
  const [vendaPagto, setVendaPagto] = useState('Pix');

  // Estados de Cadastro / Compra de Baterias
  const [novoModelo, setNovoModelo] = useState('');
  const [novaMarca, setNovaMarca] = useState('');
  const [novaAmp, setNovaAmp] = useState('');
  const [novoPreco, setNovoPreco] = useState('');
  const [novoCusto, setNovoCusto] = useState('');
  const [novoEstoque, setNovoEstoque] = useState('');
  const [novoMinEstoque, setNovoMinEstoque] = useState('');

  // Estados de Venda Passada / Retroativa
  const [passadaData, setPassadaData] = useState('');
  const [passadaCliente, setPassadaCliente] = useState('');
  const [passadaDesc, setPassadaDesc] = useState('');
  const [passadaValor, setPassadaValor] = useState('');

  // Estado de Baixa de Sucata por Peso (KG)
  const [baixaPesoKg, setBaixaPesoKg] = useState('');
  const [motivoBaixa, setMotivoBaixa] = useState('Envio para Reciclagem / Fábrica');

  // Salvar no localStorage sempre que houver alteração
  useEffect(() => {
    localStorage.setItem('imp_baterias', JSON.stringify(baterias));
  }, [baterias]);

  useEffect(() => {
    localStorage.setItem('imp_sucatas', JSON.stringify(sucatas));
  }, [sucatas]);

  useEffect(() => {
    localStorage.setItem('imp_vendas', JSON.stringify(vendas));
  }, [vendas]);

  useEffect(() => {
    localStorage.setItem('imp_dividas', JSON.stringify(dividas));
  }, [dividas]);

  const mostrarAlerta = (msg) => {
    setMensagemSucesso(msg);
    setTimeout(() => setMensagemSucesso(''), 4000);
  };

  // --- CÁLCULOS GERAIS ---
  const valorTotalEstoque = baterias.reduce((acc, b) => acc + (b.estoque * b.custo), 0);

  // Vendas de Hoje (Tipo Maquininha)
  const vendasHoje = vendas.filter(v => v.dataCurta === hojeStr || v.data.includes(hojeStr));
  const totalVendasHoje = vendasHoje.reduce((acc, v) => acc + Number(v.total || 0), 0);
  const qtdVendasHoje = vendasHoje.length;

  // Agrupar quantidade por amperagem
  const resumoAmperagem = baterias.reduce((acc, b) => {
    acc[b.amperagem] = (acc[b.amperagem] || 0) + b.estoque;
    return acc;
  }, {});

  // Total de peso de sucatas
  const pesoTotalSucatas = sucatas.reduce((acc, s) => acc + Number(s.pesoKg || 0), 0);
  const qtdTotalSucatas = sucatas.reduce((acc, s) => acc + Number(s.quantidade || 0), 0);

  // --- FUNÇÃO DE VENDA RÁPIDA ---
  const realizarVenda = (e) => {
    e.preventDefault();
    if (!vendaBateriaId) {
      alert("Selecione uma bateria para realizar a venda.");
      return;
    }

    const bat = baterias.find(b => b.id === Number(vendaBateriaId));
    if (!bat) return;

    if (bat.estoque < vendaQtd) {
      alert(`Estoque insuficiente! Restam apenas ${bat.estoque} unidades.`);
      return;
    }

    // 1. Debitar do estoque de baterias novas
    const novasBaterias = baterias.map(b => {
      if (b.id === bat.id) {
        return { ...b, estoque: b.estoque - Number(vendaQtd) };
      }
      return b;
    });
    setBaterias(novasBaterias);

    // 2. Gerar sucata automática por peso estimado
    const pesoEstimado = bat.amperagem.includes('150') || bat.amperagem.includes('100') ? 25 : 14;
    const novaSucataItem = {
      id: Date.now(),
      descricao: `Sucata ${bat.marca} ${bat.amperagem} (${bat.modelo})`,
      pesoKg: pesoEstimado * Number(vendaQtd),
      quantidade: Number(vendaQtd),
      data: new Date().toISOString().slice(0, 10)
    };
    setSucatas(prev => [novaSucataItem, ...prev]);

    // 3. Registrar Venda
    const valorTotalVenda = bat.precoVenda * Number(vendaQtd);
    const agoradate = new Date();
    const novaVendaObj = {
      id: Date.now(),
      data: agoradate.toLocaleString(),
      dataCurta: agoradate.toLocaleDateString('pt-BR'),
      cliente: vendaCliente || "Cliente Balcão",
      item: `${bat.marca} ${bat.amperagem} - ${bat.modelo}`,
      qtd: Number(vendaQtd),
      valorUnit: bat.precoVenda,
      total: valorTotalVenda,
      tipoPagto: vendaPagto,
      sucataGerada: `${vendaQtd}x ${bat.amperagem}`
    };
    setVendas(prev => [novaVendaObj, ...prev]);

    // Limpar campos e voltar para início tipo maquininha
    setVendaBateriaId('');
    setVendaQtd(1);
    setVendaCliente('');
    setAbaAtiva('inicio');
    mostrarAlerta(`Venda de R$ ${valorTotalVenda.toLocaleString('pt-BR', {minimumFractionDigits:2})} concluída com sucesso!`);
  };

  // --- BAIXA DE SUCATAS POR PESO (KG) ---
  const baixarSucataPorPeso = (e) => {
    e.preventDefault();
    const pesoBaixa = Number(baixaPesoKg);
    if (!pesoBaixa || pesoBaixa <= 0) {
      alert("Informe um peso válido em KG para dar baixa.");
      return;
    }

    if (pesoBaixa > pesoTotalSucatas) {
      alert(`O peso informado (${pesoBaixa} kg) é maior que o estoque atual de sucatas (${pesoTotalSucatas} kg).`);
      return;
    }

    let pesoRestante = pesoBaixa;
    const sucatasAtualizadas = sucatas.map(s => {
      if (pesoRestante <= 0) return s;
      if (s.pesoKg <= pesoRestante) {
        pesoRestante -= s.pesoKg;
        return { ...s, pesoKg: 0, quantidade: 0 };
      } else {
        const proporcao = pesoRestante / s.pesoKg;
        const novoPeso = s.pesoKg - pesoRestante;
        pesoRestante = 0;
        return { ...s, pesoKg: Number(novoPeso.toFixed(2)), quantidade: Math.max(0, s.quantidade - Math.ceil(proporcao)) };
      }
    }).filter(s => s.pesoKg > 0);

    setSucatas(sucatasAtualizadas);
    setBaixaPesoKg('');
    mostrarAlerta(`Baixa de ${pesoBaixa} kg realizada com sucesso!`);
  };

  const enviarDadosSefazEmail = (venda) => {
    const assunto = encodeURIComponent(`Emissão NF-e / NFC-e - Império Baterias`);
    const corpo = encodeURIComponent(
      `Solicitação de Nota Fiscal:\n\n` +
      `Data: ${venda.data}\n` +
      `Cliente: ${venda.cliente}\n` +
      `Produto: ${venda.item}\n` +
      `Quantidade: ${venda.qtd}\n` +
      `Valor Total: R$ ${venda.total}\n` +
      `Pagamento: ${venda.tipoPagto}\n`
    );
    window.open(`mailto:?subject=${assunto}&body=${corpo}`);
  };

  return (
    <div className="min-h-screen bg-slate-950 text-slate-100 flex flex-col font-sans selection:bg-amber-500 selection:text-slate-950">
      
      {/* ALERTA FLUTUANTE */}
      {mensagemSucesso && (
        <div className="fixed top-5 right-5 z-50 bg-emerald-500 text-slate-950 px-6 py-3 rounded-2xl shadow-2xl flex items-center gap-3 font-bold border border-emerald-300 animate-bounce">
          <Check className="w-6 h-6" />
          <span>{mensagemSucesso}</span>
        </div>
      )}

      {/* TOPO SIMPLES (FIXO) */}
      <header className="bg-slate-900 border-b border-slate-800 px-6 py-4 flex justify-between items-center shadow-lg">
        <button 
          onClick={() => setAbaAtiva('inicio')}
          className="flex items-center gap-3 text-left focus:outline-none"
        >
          <div className="bg-amber-500 p-2 rounded-xl text-slate-950 font-black shadow-md">
            <BatteryCharging className="w-7 h-7" />
          </div>
          <div>
            <h1 className="text-xl font-black text-amber-400 tracking-wider">IMPÉRIO BATERIAS</h1>
            <p className="text-[10px] text-slate-400 font-semibold tracking-widest uppercase">Sistema de Balcão</p>
          </div>
        </button>

        {abaAtiva !== 'inicio' && (
          <button
            onClick={() => setAbaAtiva('inicio')}
            className="flex items-center gap-2 bg-slate-800 hover:bg-slate-700 text-slate-200 px-4 py-2 rounded-xl text-sm font-bold border border-slate-700 transition-all"
          >
            <ArrowLeft className="w-4 h-4" /> Tela Inicial
          </button>
        )}
      </header>

      {/* CONTEÚDO PRINCIPAL */}
      <main className="flex-1 p-4 md:p-8 max-w-5xl mx-auto w-full flex flex-col justify-center">
        
        {/* ========================================================= */}
        {/* TELA INICIAL BÁSICA (ESTILO MAQUININHA DE CARTÃO)        */}
        {/* ========================================================= */}
        {abaAtiva === 'inicio' && (
          <div className="flex flex-col items-center justify-center space-y-8 py-4">
            
            {/* BALÃO / DISPLAY DIGITAL DE VENDAS DE HOJE */}
            <div className="w-full max-w-md bg-gradient-to-b from-slate-900 to-slate-900/90 border-2 border-amber-500/40 p-8 rounded-3xl shadow-2xl text-center relative overflow-hidden backdrop-blur-md">
              <div className="absolute top-0 left-0 right-0 h-1.5 bg-gradient-to-r from-amber-500 via-emerald-400 to-amber-500"></div>
              
              <span className="inline-block px-3 py-1 bg-amber-500/10 text-amber-400 border border-amber-500/20 rounded-full text-xs font-black tracking-wider uppercase mb-3">
                Total Vendido Hoje ({hojeStr})
              </span>

              <div className="my-2">
                <span className="text-2xl font-bold text-amber-500 mr-1">R$</span>
                <span className="text-5xl md:text-6xl font-black tracking-tight text-slate-100">
                  {totalVendasHoje.toLocaleString('pt-BR', { minimumFractionDigits: 2, maximumFractionDigits: 2 })}
                </span>
              </div>

              <div className="mt-4 pt-4 border-t border-slate-800 flex justify-center items-center gap-6 text-xs text-slate-400 font-medium">
                <span>Vendas Hoje: <strong className="text-slate-200">{qtdVendasHoje}</strong></span>
                <span>•</span>
                <span>Sucatas: <strong className="text-emerald-400">{pesoTotalSucatas.toFixed(0)} kg</strong></span>
              </div>
            </div>

            {/* BOTÕES GRANDES DE AÇÃO RÁPIDA */}
            <div className="w-full max-w-md space-y-4">
              
              {/* Botão Gigante de Nova Venda */}
              <button
                onClick={() => setAbaAtiva('venda')}
                className="w-full bg-gradient-to-r from-amber-500 to-amber-400 hover:from-amber-400 hover:to-amber-300 text-slate-950 font-black py-6 rounded-3xl text-2xl shadow-2xl shadow-amber-500/20 hover:scale-[1.02] active:scale-[0.98] transition-all flex items-center justify-center gap-3 border-2 border-amber-300 cursor-pointer"
              >
                <ShoppingCart className="w-8 h-8" />
                NOVA VENDA / VENDER ⚡
              </button>

              {/* Botão para Abrir o Menu Completo */}
              <button
                onClick={() => setAbaAtiva('menu')}
                className="w-full bg-slate-800 hover:bg-slate-700 text-slate-200 font-bold py-4 rounded-2xl text-lg border border-slate-700 shadow-xl hover:scale-[1.01] active:scale-[0.99] transition-all flex items-center justify-center gap-2 cursor-pointer"
              >
                <Menu className="w-6 h-6 text-amber-400" />
                MENU PRINCIPAL & RELATÓRIOS
              </button>

            </div>

          </div>
        )}

        {/* ========================================================= */}
        {/* TELA DE MENU PRINCIPAL (TODAS AS OPÇÕES DO SISTEMA)       */}
        {/* ========================================================= */}
        {abaAtiva === 'menu' && (
          <div className="space-y-6">
            <div className="flex justify-between items-center border-b border-slate-800 pb-4">
              <div>
                <h2 className="text-2xl font-black text-amber-400">Menu Principal</h2>
                <p className="text-xs text-slate-400">Selecione o módulo de gerenciamento desejado</p>
              </div>
            </div>

            <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4">
              {[
                { id: 'venda', label: 'Terminal de Venda', desc: 'Registrar venda com baixa de estoque', icon: ShoppingCart, color: 'text-amber-400' },
                { id: 'painel_resumo', label: 'Painel Geral', desc: 'Valor total em estoque e amperagens', icon: TrendingUp, color: 'text-blue-400' },
                { id: 'estoque', label: 'Estoque de Baterias', desc: 'Cadastrar modelos, marcas e compras', icon: Package, color: 'text-indigo-400' },
                { id: 'sucatas', label: 'Estoque de Sucatas', desc: 'Controle de peso em KG e baixa p/ fábrica', icon: Scale, color: 'text-emerald-400' },
                { id: 'historico', label: 'Histórico de Vendas', desc: 'Relatório por período e e-mail SEFAZ', icon: Calendar, color: 'text-purple-400' },
                { id: 'passadas', label: 'Vendas Passadas & Fiados', desc: 'Lançamentos antigos e dívidas', icon: FileText, color: 'text-rose-400' },
              ].map(m => {
                const Icon = m.icon;
                return (
                  <button
                    key={m.id}
                    onClick={() => setAbaAtiva(m.id)}
                    className="bg-slate-900 hover:bg-slate-800 border border-slate-800 hover:border-slate-700 p-6 rounded-3xl text-left transition-all shadow-xl group flex flex-col justify-between cursor-pointer"
                  >
                    <div>
                      <div className={`p-3 rounded-2xl bg-slate-800/80 w-fit mb-4 ${m.color} group-hover:scale-110 transition-transform`}>
                        <Icon className="w-6 h-6" />
                      </div>
                      <h3 className="font-bold text-lg text-slate-100 group-hover:text-amber-400 transition-colors">{m.label}</h3>
                      <p className="text-xs text-slate-400 mt-1">{m.desc}</p>
                    </div>
                  </button>
                );
              })}
            </div>
          </div>
        )}

        {/* ========================================================= */}
        {/* TELA DE PAINEL GERAL DE ESTOQUE E VALORES                 */}
        {/* ========================================================= */}
        {abaAtiva === 'painel_resumo' && (
          <div className="space-y-6">
            <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
              <div className="bg-slate-900 border border-slate-800 p-6 rounded-3xl shadow-xl">
                <p className="text-xs font-bold text-slate-400 uppercase tracking-wider">Valor Financeiro em Baterias</p>
                <h2 className="text-3xl font-black text-amber-400 mt-1">
                  R$ {valorTotalEstoque.toLocaleString('pt-BR', { minimumFractionDigits: 2 })}
                </h2>
                <span className="text-xs text-slate-500 mt-2 block">Custo total do estoque novo</span>
              </div>

              <div className="bg-slate-900 border border-slate-800 p-6 rounded-3xl shadow-xl">
                <p className="text-xs font-bold text-slate-400 uppercase tracking-wider">Estoque Atual de Sucatas</p>
                <h2 className="text-3xl font-black text-emerald-400 mt-1">
                  {pesoTotalSucatas.toFixed(1)} kg
                </h2>
                <span className="text-xs text-slate-500 mt-2 block">({qtdTotalSucatas} baterias prontas para troca)</span>
              </div>
            </div>

            <div className="bg-slate-900 border border-slate-800 p-6 rounded-3xl shadow-xl">
              <h3 className="text-lg font-bold text-slate-200 mb-4">Estoque por Amperagem</h3>
              <div className="grid grid-cols-2 sm:grid-cols-4 gap-4">
                {Object.entries(resumoAmperagem).map(([amp, qtd]) => (
                  <div key={amp} className="bg-slate-950 p-4 rounded-2xl border border-slate-800 text-center">
                    <span className="text-xs text-slate-400 font-semibold">{amp}</span>
                    <p className="text-2xl font-black text-slate-100 mt-1">{qtd} <span className="text-xs font-normal text-slate-400">un</span></p>
                  </div>
                ))}
              </div>
            </div>
          </div>
        )}

        {/* ========================================================= */}
        {/* ABA: NOVA VENDA RÁPIDA                                    */}
        {/* ========================================================= */}
        {abaAtiva === 'venda' && (
          <div className="max-w-2xl mx-auto bg-slate-900 border border-slate-800 p-8 rounded-3xl shadow-2xl w-full">
            <h2 className="text-2xl font-black text-amber-400 mb-1 flex items-center gap-3">
              <ShoppingCart className="w-7 h-7" />
              Terminal de Venda
            </h2>
            <p className="text-xs text-slate-400 mb-6">Debita a bateria nova e gera a sucata em KG automaticamente.</p>

            <form onSubmit={realizarVenda} className="space-y-6">
              <div>
                <label className="block text-sm font-bold text-slate-300 mb-2">Selecione a Bateria *</label>
                <select
                  value={vendaBateriaId}
                  onChange={(e) => setVendaBateriaId(e.target.value)}
                  className="w-full bg-slate-950 border border-slate-800 rounded-2xl px-4 py-4 text-slate-100 font-semibold focus:outline-none focus:border-amber-500 text-base"
                  required
                >
                  <option value="">-- Escolha o modelo / amperagem --</option>
                  {baterias.map(b => (
                    <option key={b.id} value={b.id}>
                      {b.marca} {b.amperagem} - {b.modelo} | Preço: R$ {b.precoVenda} (Estoque: {b.estoque})
                    </option>
                  ))}
                </select>
              </div>

              <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
                <div>
                  <label className="block text-sm font-bold text-slate-300 mb-2">Quantidade</label>
                  <input
                    type="number"
                    min="1"
                    value={vendaQtd}
                    onChange={(e) => setVendaQtd(e.target.value)}
                    className="w-full bg-slate-950 border border-slate-800 rounded-2xl px-4 py-4 text-slate-100 font-semibold focus:outline-none focus:border-amber-500 text-base"
                    required
                  />
                </div>
                <div>
                  <label className="block text-sm font-bold text-slate-300 mb-2">Forma de Pagamento</label>
                  <select
                    value={vendaPagto}
                    onChange={(e) => setVendaPagto(e.target.value)}
                    className="w-full bg-slate-950 border border-slate-800 rounded-2xl px-4 py-4 text-slate-100 font-semibold focus:outline-none focus:border-amber-500 text-base"
                  >
                    <option value="Pix">Pix</option>
                    <option value="Dinheiro">Dinheiro</option>
                    <option value="Cartão Crédito">Cartão Crédito</option>
                    <option value="Cartão Débito">Cartão Débito</option>
                    <option value="Fiado / A Receber">Fiado / A Receber</option>
                  </select>
                </div>
              </div>

              <div>
                <label className="block text-sm font-bold text-slate-300 mb-2">Nome do Cliente (Opcional)</label>
                <input
                  type="text"
                  placeholder="Ex: João da Oficina"
                  value={vendaCliente}
                  onChange={(e) => setVendaCliente(e.target.value)}
                  className="w-full bg-slate-950 border border-slate-800 rounded-2xl px-4 py-4 text-slate-100 placeholder-slate-600 focus:outline-none focus:border-amber-500 text-base"
                />
              </div>

              <button
                type="submit"
                className="w-full bg-amber-500 hover:bg-amber-400 text-slate-950 font-black py-5 rounded-2xl text-xl shadow-xl transition-all flex items-center justify-center gap-2 cursor-pointer"
              >
                <CheckCircle className="w-6 h-6" />
                CONCLUIR VENDA
              </button>
            </form>
          </div>
        )}

        {/* ========================================================= */}
        {/* ABA: ESTOQUE DE BATERIAS NOVAS                            */}
        {/* ========================================================= */}
        {abaAtiva === 'estoque' && (
          <div className="space-y-6">
            <div className="bg-slate-900 border border-slate-800 p-6 rounded-3xl shadow-xl">
              <h2 className="text-xl font-black text-amber-400 mb-4">Cadastrar / Entrada de Baterias</h2>
              <form onSubmit={(e) => {
                e.preventDefault();
                if (!novoModelo || !novaMarca || !novaAmp || !novoPreco || !novoEstoque) {
                  alert("Preencha todos os campos obrigatórios.");
                  return;
                }
                const nova = {
                  id: Date.now(),
                  modelo: novoModelo,
                  marca: novaMarca,
                  amperagem: novaAmp,
                  precoVenda: Number(novoPreco),
                  custo: Number(novoCusto || 0),
                  estoque: Number(novoEstoque),
                  minEstoque: Number(novoMinEstoque || 2)
                };
                setBaterias([...baterias, nova]);
                setNovoModelo(''); setNovaMarca(''); setNovaAmp(''); setNovoPreco(''); setNovoCusto(''); setNovoEstoque(''); setNovoMinEstoque('');
                mostrarAlerta("Bateria cadastrada com sucesso!");
              }} className="grid grid-cols-1 md:grid-cols-4 gap-4">
                <input type="text" placeholder="Marca (Ex: Moura)" value={novaMarca} onChange={e => setNovaMarca(e.target.value)} className="bg-slate-950 border border-slate-800 p-3 rounded-xl text-sm" required />
                <input type="text" placeholder="Amperagem (Ex: 60 Ah)" value={novaAmp} onChange={e => setNovaAmp(e.target.value)} className="bg-slate-950 border border-slate-800 p-3 rounded-xl text-sm" required />
                <input type="text" placeholder="Modelo (Ex: Selada)" value={novoModelo} onChange={e => setNovoModelo(e.target.value)} className="bg-slate-950 border border-slate-800 p-3 rounded-xl text-sm" required />
                <input type="number" placeholder="Qtd Inicial" value={novoEstoque} onChange={e => setNovoEstoque(e.target.value)} className="bg-slate-950 border border-slate-800 p-3 rounded-xl text-sm" required />
                <input type="number" placeholder="Custo (R$)" value={novoCusto} onChange={e => setNovoCusto(e.target.value)} className="bg-slate-950 border border-slate-800 p-3 rounded-xl text-sm" />
                <input type="number" placeholder="Preço Venda (R$)" value={novoPreco} onChange={e => setNovoPreco(e.target.value)} className="bg-slate-950 border border-slate-800 p-3 rounded-xl text-sm" required />
                <input type="number" placeholder="Estoque Mínimo" value={novoMinEstoque} onChange={e => setNovoMinEstoque(e.target.value)} className="bg-slate-950 border border-slate-800 p-3 rounded-xl text-sm" />
                <button type="submit" className="bg-amber-500 hover:bg-amber-400 text-slate-950 font-bold p-3 rounded-xl shadow-lg">Cadastrar</button>
              </form>
            </div>

            <div className="bg-slate-900 border border-slate-800 rounded-3xl shadow-xl overflow-hidden">
              <div className="p-6 border-b border-slate-800">
                <h3 className="text-lg font-bold text-slate-100">Baterias Novas Cadastradas</h3>
              </div>
              <div className="overflow-x-auto">
                <table className="w-full text-left border-collapse">
                  <thead>
                    <tr className="bg-slate-950 text-slate-400 text-xs uppercase tracking-wider">
                      <th className="p-4">Marca / Amperagem</th>
                      <th className="p-4">Modelo</th>
                      <th className="p-4">Preço Venda</th>
                      <th className="p-4">Estoque</th>
                      <th className="p-4 text-right">Ações</th>
                    </tr>
                  </thead>
                  <tbody className="divide-y divide-slate-800 text-sm">
                    {baterias.map(b => (
                      <tr key={b.id} className="hover:bg-slate-800/50">
                        <td className="p-4 font-bold text-amber-400">{b.marca} {b.amperagem}</td>
                        <td className="p-4 text-slate-300">{b.modelo}</td>
                        <td className="p-4 text-slate-300">R$ {b.precoVenda}</td>
                        <td className="p-4 font-bold">{b.estoque} un</td>
                        <td className="p-4 text-right">
                          <button 
                            onClick={() => {
                              if (confirm("Excluir esta bateria do cadastro?")) {
                                setBaterias(baterias.filter(item => item.id !== b.id));
                              }
                            }}
                            className="bg-rose-500/20 text-rose-400 hover:bg-rose-500 hover:text-white p-2 rounded-lg transition-all"
                          >
                            <Trash2 className="w-4 h-4" />
                          </button>
                        </td>
                      </tr>
                    ))}
                  </tbody>
                </table>
              </div>
            </div>
          </div>
        )}

        {/* ========================================================= */}
        {/* ABA: SUCATAS (CONTROLE POR PESO)                          */}
        {/* ========================================================= */}
        {abaAtiva === 'sucatas' && (
          <div className="space-y-6">
            <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
              <div className="bg-slate-900 border border-slate-800 p-6 rounded-3xl shadow-xl flex flex-col justify-between">
                <div>
                  <h3 className="text-lg font-bold text-emerald-400 mb-2">Balanço de Sucatas</h3>
                  <p className="text-xs text-slate-400">Total de chumbo acumulado das vendas realizadas no balcão.</p>
                  
                  <div className="mt-6 grid grid-cols-2 gap-4">
                    <div className="bg-slate-950 p-4 rounded-2xl border border-slate-800 text-center">
                      <span className="text-xs text-slate-400">Peso Total</span>
                      <p className="text-3xl font-black text-emerald-400 mt-1">{pesoTotalSucatas.toFixed(1)} kg</p>
                    </div>
                    <div className="bg-slate-950 p-4 rounded-2xl border border-slate-800 text-center">
                      <span className="text-xs text-slate-400">Qtd Itens</span>
                      <p className="text-3xl font-black text-slate-100 mt-1">{qtdTotalSucatas} un</p>
                    </div>
                  </div>
                </div>
              </div>

              <div className="bg-slate-900 border border-slate-800 p-6 rounded-3xl shadow-xl">
                <h3 className="text-lg font-bold text-amber-400 mb-2">Dar Baixa em Sucata Entregue</h3>
                <form onSubmit={baixarSucataPorPeso} className="space-y-4">
                  <div>
                    <label className="block text-xs font-bold text-slate-300 mb-1">Peso Entregue na Troca (KG)</label>
                    <input
                      type="number"
                      step="0.1"
                      placeholder="Ex: 120.5"
                      value={baixaPesoKg}
                      onChange={e => setBaixaPesoKg(e.target.value)}
                      className="w-full bg-slate-950 border border-slate-800 p-3 rounded-2xl text-slate-100 font-semibold"
                      required
                    />
                  </div>
                  <button type="submit" className="w-full bg-emerald-600 hover:bg-emerald-500 text-white font-bold py-3 rounded-2xl shadow-lg transition-all">
                    BAIXAR PESO DO ESTOQUE
                  </button>
                </form>
              </div>
            </div>
          </div>
        )}

        {/* ========================================================= */}
        {/* ABA: HISTÓRICO DE VENDAS                                  */}
        {/* ========================================================= */}
        {abaAtiva === 'historico' && (
          <div className="bg-slate-900 border border-slate-800 rounded-3xl shadow-xl overflow-hidden">
            <div className="p-6 border-b border-slate-800 flex justify-between items-center">
              <h3 className="text-lg font-bold text-slate-100">Histórico de Vendas</h3>
              <span className="text-xs bg-slate-950 text-slate-400 px-3 py-1.5 rounded-lg">
                Total: {vendas.length} vendas
              </span>
            </div>
            <div className="overflow-x-auto">
              <table className="w-full text-left border-collapse">
                <thead>
                  <tr className="bg-slate-950 text-slate-400 text-xs uppercase tracking-wider">
                    <th className="p-4">Data/Hora</th>
                    <th className="p-4">Cliente</th>
                    <th className="p-4">Produto</th>
                    <th className="p-4">Total</th>
                    <th className="p-4">Pagto</th>
                    <th className="p-4 text-center">E-mail SEFAZ</th>
                  </tr>
                </thead>
                <tbody className="divide-y divide-slate-800 text-sm">
                  {vendas.map(v => (
                    <tr key={v.id} className="hover:bg-slate-800/50">
                      <td className="p-4 text-slate-400 text-xs">{v.data}</td>
                      <td className="p-4 font-bold text-slate-200">{v.cliente}</td>
                      <td className="p-4 text-amber-400">{v.item}</td>
                      <td className="p-4 font-bold text-emerald-400">R$ {v.total.toLocaleString('pt-BR', { minimumFractionDigits: 2 })}</td>
                      <td className="p-4"><span className="bg-slate-950 px-2 py-1 rounded-md text-xs">{v.tipoPagto}</span></td>
                      <td className="p-4 text-center">
                        <button
                          onClick={() => enviarDadosSefazEmail(v)}
                          className="bg-amber-500/20 text-amber-400 hover:bg-amber-500 hover:text-slate-950 font-bold text-xs px-3 py-1.5 rounded-xl transition-all border border-amber-500/30"
                        >
                          Enviar Dados
                        </button>
                      </td>
                    </tr>
                  ))}
                </tbody>
              </table>
            </div>
          </div>
        )}

        {/* ========================================================= */}
        {/* ABA: PASSADAS E DÍVIDAS                                   */}
        {/* ========================================================= */}
        {abaAtiva === 'passadas' && (
          <div className="space-y-6">
            <div className="bg-slate-900 border border-slate-800 p-6 rounded-3xl shadow-xl">
              <h3 className="text-lg font-black text-amber-400 mb-2">Lançar Venda Passada / Retroativa</h3>
              <form onSubmit={(e) => {
                e.preventDefault();
                if (!passadaCliente || !passadaDesc || !passadaValor) return;
                const dataFormatada = passadaData ? new Date(passadaData + 'T12:00:00').toLocaleDateString('pt-BR') : hojeStr;
                const novaP = {
                  id: Date.now(),
                  data: dataFormatada,
                  dataCurta: dataFormatada,
                  cliente: passadaCliente,
                  item: passadaDesc,
                  qtd: 1,
                  valorUnit: Number(passadaValor),
                  total: Number(passadaValor),
                  tipoPagto: "Retroativo",
                  sucataGerada: "Manual"
                };
                setVendas([novaP, ...vendas]);
                setPassadaCliente(''); setPassadaDesc(''); setPassadaValor(''); setPassadaData('');
                mostrarAlerta("Venda passada lançada!");
              }} className="grid grid-cols-1 md:grid-cols-4 gap-4">
                <input type="date" value={passadaData} onChange={e => setPassadaData(e.target.value)} className="bg-slate-950 border border-slate-800 p-3 rounded-xl text-sm" required />
                <input type="text" placeholder="Cliente" value={passadaCliente} onChange={e => setPassadaCliente(e.target.value)} className="bg-slate-950 border border-slate-800 p-3 rounded-xl text-sm" required />
                <input type="text" placeholder="Descrição da Venda" value={passadaDesc} onChange={e => setPassadaDesc(e.target.value)} className="bg-slate-950 border border-slate-800 p-3 rounded-xl text-sm" required />
                <input type="number" placeholder="Valor (R$)" value={passadaValor} onChange={e => setPassadaValor(e.target.value)} className="bg-slate-950 border border-slate-800 p-3 rounded-xl text-sm" required />
                <button type="submit" className="md:col-span-4 bg-amber-500 hover:bg-amber-400 text-slate-950 font-bold p-3 rounded-xl shadow-lg">Salvar Lançamento</button>
              </form>
            </div>
          </div>
        )}

      </main>
    </div>
  );
}
