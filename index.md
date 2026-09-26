[セルフマネジメント（Daily _ Monthly _ Annual goal）.html](https://github.com/user-attachments/files/32680152/Daily._.Monthly._.Annual.goal.html)
<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<title>現在地トラッカー</title>
<style>
  :root{
    --bg:#1c1c1c; --panel:#242424; --panel2:#2b2b2b; --text:#e4e4e2; --muted:#9a9a96;
    --accent:#c98a5e; --accent2:#6f9e8c; --border:#3a3a37; --track:#3a3a37; --danger:#c96a5e;
  }
  @media (prefers-color-scheme: light){
    :root:not([data-theme="dark"]){
      --bg:#f6f4f0; --panel:#ffffff; --panel2:#fbf9f5; --text:#2a2925; --muted:#7a776f;
      --border:#e4e0d8; --track:#e8e4dc;
    }
  }
  :root[data-theme="light"]{
    --bg:#f6f4f0; --panel:#ffffff; --panel2:#fbf9f5; --text:#2a2925; --muted:#7a776f;
    --border:#e4e0d8; --track:#e8e4dc;
  }
  html,body{height:100%;}
  *{box-sizing:border-box;}
  body{
    margin:0; background:var(--bg); color:var(--text);
    font-family:"Hiragino Sans","Segoe UI",system-ui,sans-serif;
    padding-top:env(safe-area-inset-top,0px); padding-bottom:env(safe-area-inset-bottom,0px);
    line-height:1.5;
  }
  .wrap{max-width:780px; margin:0 auto; padding:28px 18px 60px;}
  .topbar{display:flex; justify-content:space-between; align-items:flex-start; gap:12px;}
  h1{font-size:1.5rem; margin:0 0 4px;}
  .sub{color:var(--muted); font-size:.85rem; margin-bottom:22px;}
  .editToggle{flex-shrink:0; background:var(--panel); border:1px solid var(--border); color:var(--text); font-size:.78rem; padding:7px 12px; border-radius:7px; cursor:pointer;}
  .editToggle.on{background:var(--accent); border-color:var(--accent); color:#fff;}

  .overall{background:var(--panel); border:1px solid var(--border); border-radius:10px; padding:18px 20px; margin-bottom:26px;}
  .overall-top{display:flex; justify-content:space-between; align-items:baseline; margin-bottom:8px;}
  .overall-num{font-size:2rem; font-weight:700; color:var(--accent);}
  .bigtrack{height:10px; background:var(--track); border-radius:5px; overflow:hidden;}
  .bigfill{height:100%; background:var(--accent); transition:width .2s;}

  .card{background:var(--panel); border:1px solid var(--border); border-radius:10px; padding:16px 18px; margin-bottom:14px;}
  .card>.chead{display:flex; justify-content:space-between; align-items:baseline; gap:8px;}
  .card>.chead h2{font-size:1.02rem; margin:0; flex:1;}
  .card>.chead .pct{color:var(--muted); font-size:.85rem; font-weight:600; white-space:nowrap;}
  .card>.track{height:6px; background:var(--track); border-radius:3px; overflow:hidden; margin:8px 0 14px;}
  .card>.track>.fill{height:100%; background:var(--accent2);}

  .group{margin:6px 0; padding-left:10px; border-left:2px solid var(--border);}
  .ghead{display:flex; justify-content:space-between; align-items:baseline; font-size:.88rem; cursor:pointer; padding:4px 0; user-select:none; gap:8px;}
  .ghead .gname{display:flex; align-items:baseline; gap:6px; flex:1; min-width:0;}
  .ghead .arrow{color:var(--muted); font-size:.72rem; transition:transform .15s; display:inline-block; flex-shrink:0;}
  .group.open>.ghead .arrow{transform:rotate(90deg);}
  .ghead .gpct{color:var(--muted); font-size:.78rem; white-space:nowrap;}
  .gtrack{height:4px; background:var(--track); border-radius:2px; overflow:hidden; margin:2px 0 4px;}
  .gtrack>.gfill{height:100%; background:var(--accent2);}
  .gchildren{display:none;}
  .group.open>.gchildren{display:block;}
  .depth1>.ghead{font-weight:600;}
  .depth2>.ghead{font-weight:500; color:var(--text);}
  .depth3>.ghead{font-weight:400; color:var(--muted);}

  .item-row{padding:7px 0 7px 10px; display:flex; align-items:center; gap:8px;}
  .item-body{flex:1; min-width:0;}
  .item-top{display:flex; justify-content:space-between; align-items:baseline; font-size:.82rem; gap:8px;}
  .item-pct{color:var(--muted); font-size:.78rem; min-width:34px; text-align:right;}
  input[type=range]{
    width:100%; margin-top:4px; -webkit-appearance:none; height:4px; border-radius:2px;
    background:var(--track); accent-color:var(--accent2);
  }

  .ebtn{flex-shrink:0; border:none; background:none; color:var(--muted); font-size:.85rem; cursor:pointer; padding:2px 4px; border-radius:4px;}
  .ebtn:hover{background:var(--panel2);}
  .ebtn.del{color:var(--danger);}
  .addrow{margin:6px 0 4px 10px;}
  .addbtn{background:none; border:1px dashed var(--border); color:var(--muted); font-size:.76rem; padding:4px 10px; border-radius:6px; cursor:pointer; margin-right:6px; margin-bottom:4px;}
  .addbtn:hover{color:var(--text); border-color:var(--accent2);}
  .addCardBtn{display:block; width:100%; margin-top:4px; background:none; border:1px dashed var(--border); color:var(--muted); font-size:.85rem; padding:10px; border-radius:10px; cursor:pointer;}
  .addCardBtn:hover{color:var(--text); border-color:var(--accent2);}

  details.exp{background:var(--panel); border:1px solid var(--border); border-radius:10px; padding:14px 18px; margin-top:14px;}
  details.exp summary{cursor:pointer; font-size:1.02rem; font-weight:600;}
  .exp ul{margin:10px 0 0; padding-left:20px; font-size:.86rem; color:var(--muted);}
  .exp ul li{margin-bottom:3px;}

  .reset{display:block; margin:16px auto 0; background:none; border:1px solid var(--border); color:var(--muted); font-size:.78rem; padding:6px 14px; border-radius:6px; cursor:pointer;}

  .tabs{display:flex; gap:6px; margin-bottom:20px; border-bottom:1px solid var(--border);}
  .tabbtn{background:none; border:none; color:var(--muted); font-size:.86rem; padding:9px 4px; margin-right:14px; cursor:pointer; border-bottom:2px solid transparent;}
  .tabbtn.active{color:var(--text); border-bottom-color:var(--accent); font-weight:600;}
  .tabpanel{display:none;}
  .tabpanel.active{display:block;}

  .daynav{display:flex; align-items:center; gap:10px; margin-bottom:16px;}
  .daylabel{flex:1; text-align:center; font-size:.98rem; font-weight:600;}
  .navbtn{background:none; border:1px solid var(--border); color:var(--text); border-radius:6px; padding:5px 10px; cursor:pointer; font-size:.85rem;}
  .todayBtn{background:none; border:1px solid var(--border); color:var(--muted); border-radius:6px; padding:5px 10px; cursor:pointer; font-size:.78rem;}
  .ftime{color:var(--muted); font-size:.8rem; margin-top:8px;}

  .addTaskRow{display:flex; flex-wrap:wrap; gap:8px; margin-top:8px;}
  .addTaskRow input[type=text]{flex:2; min-width:140px;}
  .addTaskRow select{flex:1; min-width:110px;}
  .addTaskRow input[type=number]{width:90px;}
  .addTaskRow input,.addTaskRow select{padding:7px 9px; border-radius:6px; border:1px solid var(--border); background:var(--panel2); color:var(--text); font-size:.85rem;}
  .addTaskRow .addbtn{border-style:solid;}

  .pgroup{margin-bottom:14px;}
  .pgroup h3{font-size:.82rem; color:var(--muted); margin:0 0 6px; display:flex; align-items:center; gap:6px;}
  .pbadge{display:inline-block; width:20px; height:20px; border-radius:5px; color:#fff; font-size:.72rem; font-weight:700; text-align:center; line-height:20px;}
  .pbadge.A{background:#c9695e;} .pbadge.B{background:#c98a5e;} .pbadge.C{background:#6f9e8c;}

  .task{background:var(--panel); border:1px solid var(--border); border-radius:8px; padding:10px 12px; margin-bottom:8px; display:flex; align-items:center; gap:10px;}
  .task.done{opacity:.55;}
  .task.done .ttext{text-decoration:line-through;}
  .task input[type=checkbox]{width:18px; height:18px; accent-color:var(--accent2); flex-shrink:0;}
  .task .tbody{flex:1; min-width:0;}
  .task .ttext{font-size:.9rem; word-break:break-word;}
  .task .tmeta{display:flex; gap:10px; align-items:center; margin-top:5px; font-size:.76rem; color:var(--muted);}
  .task .tmeta input{width:60px; padding:3px 6px; border-radius:5px; border:1px solid var(--border); background:var(--panel2); color:var(--text); font-size:.76rem;}
  .noTask{color:var(--muted); font-size:.85rem; padding:8px 0;}

  .mrow{background:var(--panel); border:1px solid var(--border); border-radius:8px; padding:9px 12px; margin-bottom:6px; display:flex; align-items:center; gap:10px; font-size:.85rem;}
  .mrow .mdate{width:78px; flex-shrink:0; color:var(--muted);}
  .mrow .mstat{flex:1;}
  .mrow .mbar{height:4px; background:var(--track); border-radius:2px; overflow:hidden; margin-top:4px;}
  .mrow .mbar>div{height:100%; background:var(--accent2);}
  .mrow .mmin{width:64px; text-align:right; color:var(--muted); flex-shrink:0;}

  .tlinktime{font-size:.72rem; color:var(--accent2); margin-top:2px;}

  .chipRow{display:flex; flex-wrap:wrap; gap:7px; margin:10px 0 2px;}
  .chip{background:var(--panel2); border:1px solid var(--border); border-radius:16px; padding:6px 12px; font-size:.8rem; cursor:pointer; display:flex; align-items:center; gap:6px;}
  .chip:hover{border-color:var(--accent2);}
  .chip .cx{color:var(--muted); font-size:.7rem;}
  .chipLabel{color:var(--muted); font-size:.76rem; margin-top:12px; margin-bottom:2px;}
  .tmpBtn{background:none; border:1px dashed var(--border); color:var(--muted); font-size:.8rem; padding:7px 12px; border-radius:6px; cursor:pointer;}
  .gcalLink{text-decoration:none;}

  .addTaskRow input,.addTaskRow select,.task .tmeta input{font-size:16px !important;}

  .goalrow{background:var(--panel2); border:1px solid var(--border); border-radius:8px; padding:9px 12px; margin-bottom:6px; display:flex; align-items:center; gap:10px; font-size:.85rem;}
  .goalrow .gtitle{flex:1; min-width:0;}
  .goalrow .gdeadline{color:var(--muted); font-size:.76rem; white-space:nowrap;}
  .catbadge{background:var(--track); color:var(--muted); font-size:.72rem; padding:2px 8px; border-radius:10px; white-space:nowrap;}
  .mtarget{width:100%; padding:7px 9px; border-radius:6px; border:1px solid var(--border); background:var(--panel2); color:var(--text); font-size:16px; margin-top:4px;}
  .mgoalrow{margin-bottom:12px;}
  .mgoalrow .gname{font-size:.85rem; font-weight:600; margin-bottom:2px;}
  textarea.mreflect{width:100%; min-height:90px; padding:9px; border-radius:6px; border:1px solid var(--border); background:var(--panel2); color:var(--text); font-size:16px; font-family:inherit; margin-top:6px; resize:vertical;}
</style>
</head>
<body>
<div class="wrap">
  <h1>📍 セルフマネジメント</h1>

  <div class="tabs">
    <button class="tabbtn active" data-tab="franklin">Daily</button>
    <button class="tabbtn" data-tab="monthly">Monthly</button>
    <button class="tabbtn" data-tab="genzaichi">Annual goal</button>
  </div>

  <div id="tab-genzaichi" class="tabpanel">
  <div class="topbar">
    <p class="sub">資格名をタップすると内訳が開きます。編集モードで項目の追加・修正・削除ができます。</p>
    <button class="editToggle" id="editToggle">✏️ 編集</button>
  </div>

  <div class="card" id="goalsCard">
    <div class="chead"><h2>目標</h2></div>
    <div class="addTaskRow">
      <select id="goalTitle"></select>
      <input type="date" id="goalDeadline">
      <button id="addGoalBtn" class="addbtn">＋ 追加</button>
    </div>
    <div id="goalList"></div>
  </div>

  <div class="overall">
    <div class="overall-top">
      <span>全体の進捗</span>
      <span class="overall-num" id="overallPct">0%</span>
    </div>
    <div class="bigtrack"><div class="bigfill" id="overallFill" style="width:0%"></div></div>
  </div>

  <section id="sections"></section>
  <button class="addCardBtn" id="addCardBtn" style="display:none;">＋ 新しいカードを追加</button>

  <details class="exp">
    <summary>これまでの経歴（EC・販売・マーケティング関連）</summary>
    <ul>
      <li>EC販売の実務経験</li>
      <li>半年で5,000万円売上の経験</li>
      <li>中古販売で月60万円程度の実績</li>
      <li>市場分析</li>
      <li>顧客分析</li>
      <li>ペルソナ／ターゲット設定</li>
      <li>商品分析・商品詳細の把握</li>
      <li>価格設定</li>
      <li>販売方法の設計</li>
      <li>SEO分析</li>
      <li>競合調査</li>
      <li>顧客調査</li>
      <li>広告</li>
      <li>顧客への訴求方法</li>
      <li>ブランド価値向上</li>
      <li>「売れない原因」を商品・顧客・競合・訴求・導線などに分解して考える</li>
    </ul>
  </details>

  <button class="reset" id="resetBtn">すべてリセット（内容も含む）</button>
  </div>

  <div id="tab-franklin" class="tabpanel active">
    <div class="daynav">
      <button class="navbtn" id="prevDay">◀</button>
      <div class="daylabel" id="dayLabel">9月23日（水）</div>
      <button class="navbtn" id="nextDay">▶</button>
      <button class="todayBtn" id="todayBtn">今日</button>
    </div>

    <div class="overall">
      <div class="overall-top"><span>本日の完了</span><span class="overall-num" id="fPct">0%</span></div>
      <div class="bigtrack"><div class="bigfill" id="fFill" style="width:0%"></div></div>
      <div class="ftime" id="fTime">予定 0分 ／ 実績 0分</div>
    </div>

    <div class="card">
      <div class="chead"><h2>タスクを登録</h2></div>
      <div class="addTaskRow">
        <input type="text" id="taskText" placeholder="タスク内容">
        <select id="taskPriority">
          <option value="A">A（重要・優先度高）</option>
          <option value="B">B（普通）</option>
          <option value="C">C（余裕があれば）</option>
        </select>
        <input type="number" id="taskEst" placeholder="予定(分)" min="0" step="5">
        <select id="taskLink"><option value="">現在地トラッカーと紐付け（任意）</option></select>
        <button id="addTaskBtn" class="addbtn">＋ 追加</button>
        <button id="saveTemplateBtn" class="tmpBtn">☆ よく使うタスクに登録</button>
      </div>
      <div class="chipLabel">よく使うタスク（タップで即登録）</div>
      <div class="chipRow" id="templateChips"></div>
    </div>

    <div id="taskGroups"></div>
  </div>

  <div id="tab-monthly" class="tabpanel">
    <div class="daynav">
      <button class="navbtn" id="prevMonth">◀</button>
      <div class="daylabel" id="monthLabel">2026年9月</div>
      <button class="navbtn" id="nextMonth">▶</button>
      <button class="todayBtn" id="thisMonthBtn">今月</button>
    </div>
    <div class="card">
      <div class="chead"><h2>今月の目標</h2></div>
      <div class="addTaskRow">
        <input type="text" id="mgTitle" placeholder="今月の目標（例：SQLを基礎から固める）">
        <select id="mgLinkGoal"><option value="">Annual goalと紐付け（任意）</option></select>
        <input type="number" id="mgTarget" placeholder="目標達成率%" min="0" max="100" step="5">
        <button id="addMGoalBtn" class="addbtn">＋ 追加</button>
      </div>
      <div id="monthGoalTargets"></div>
    </div>

    <div class="overall">
      <div class="overall-top"><span>月間実績時間</span><span class="overall-num" id="mTime">0時間</span></div>
      <div class="ftime" id="mSummary">タスク 0件／完了 0件（平均達成率 0%）</div>
    </div>
    <div id="monthList"></div>

    <div class="card">
      <div class="chead"><h2>振り返り</h2></div>
      <textarea class="mreflect" id="monthReflect" placeholder="今月の振り返りを書く"></textarea>
    </div>
  </div>
</div>

<script>
(function(){
  const SEED = [
    {
      id:"it",
      title:"IT資格取得",
      items:[
        {name:"基本情報技術者試験", children:[
          {name:"①テクノロジ系（コンピュータの技術・仕組み）", children:[
            {name:"基礎理論", children:["離散数学","応用数学","アルゴリズムとプログラミング"]},
            {name:"コンピュータシステム", children:["プロセッサ、メモリ、バスなどのコンピュータ構成要素","OSやミドルウェアなどのソフトウェア","ハードウェア"]},
            {name:"技術要素", children:["ヒューマンインタフェース","マルチメディア","データベース","ネットワーク","セキュリティ"]},
            {name:"開発技術", children:["システム開発ライフサイクル","要件定義","設計","プログラミング","テスト","ソフトウェア開発管理"]}
          ]},
          {name:"②マネジメント系（プロジェクトやサービスの管理）", children:[
            {name:"プロジェクトマネジメント", children:["プロジェクト計画","工程管理","コスト管理","リスク管理"]},
            {name:"サービスマネジメント", children:["サービスレベル管理","可用性管理","ITサービス運用"]},
            {name:"システム監査", children:["システム監査の計画","実施","評価"]}
          ]},
          {name:"③ストラテジ系（経営・ビジネス・法務）", children:[
            {name:"システム戦略", children:["システム化計画","要件定義","調達計画"]},
            {name:"経営戦略・マネジメント", children:["経営理念（MVV）","SWOT分析","マーケティング","ERP"]},
            {name:"企業と法務", children:["企業活動","組織論","会計","財務","知的財産権（著作権法・特許法など）","労働法規"]}
          ]}
        ]},
        {name:"G検定", children:[
          {name:"①人工知能（AI）とは", children:["人工知能の定義","歴史","主要なアプローチ","人工知能分野で議論されている問題（シンギュラリティ、トレイ・テストなど）"]},
          {name:"②人工知能をめぐる動向", children:["探索・推論","知識表現","第1次・第2次ブーム（エキスパートシステムなど）の背景と限界","機械学習・ディープラーニングに至る流れ"]},
          {name:"③機械学習の具体的手法", children:["教師あり学習（回帰、分類）","教師なし学習（クラスタリング、次元削減）強化学習","評価指標（混同行列、精度、適合率、再現率など）"]},
          {name:"④ディープラーニングの概要", children:["ニューラルネットワークの仕組み（活性化関数、誤差逆伝播法、勾配消失問題）","隠れ層の種類と発展（CNN、RNN、LSTM、Transformerなど）"]},
          {name:"⑤ディープラーニングの手法・応用", children:["画像認識（物体検出、セマンティックセグメンテーション）","自然言語処理（Word2Vec、大規模言語モデルなど）","生成AI（GAN、拡散モデル）","マルチモーダル","転移学習"]},
          {name:"⑥AIプロジェクトとデータ・数学基礎", children:["データの収集・前処理","AIプロジェクトの進め方（PoC、アジャイル開発）","数理・統計の基礎（確率・統計、線形代数、微分などの基本知識）"]},
          {name:"⑦法律・倫理・社会問題", children:["AIに関する著作権","契約","プライバシー","データ利活用に関する法規制","AI倫理","ガバナンス","ガイドライン（国内外の動向"]}
        ]},
        {name:"SQL", children:["「データ操作」","「データ定義」","「データ制御」"]},
        {name:"PM系資格", children:[
          {name:"プロジェクトマネジメント（最重要・レベル4）", children:[
            "プロジェクト統合マネジメント（プロジェクト憲章、プロジェクト管理計画、変更管理、クローズアウト）",
            "プロジェクトスコープマネジメント（要件定義、スコープ定義、WBS作成、スコープ検証・コントロール）",
            "プロジェクトスケジュールマネジメント（アクティビティ定義、アローダイアグラム/PERT、クリティカルパス法、CCPM、進捗管理）",
            "プロジェクトコストマネジメント（コスト見積もり、類推見積、ボトムアップ見積、ファンクションポイント法、予算設定、EVM/アーンドバリューマネジメント）",
            "プロジェクト品質マネジメント（品質計画、品質保証、品質管理、QC七つ道具、レビュー、テスト管理）",
            "プロジェクト資源マネジメント（チーム編成、要員管理、役割・責任、チーム育成、コンフリクトマネジメント）",
            "プロジェクトコミュニケーションマネジメント（コミュニケーション計画、情報配布、ステークホルダー報告）",
            "プロジェクトリスクマネジメント（リスク特定、定性的リスク分析、定量リスク分析、リスク対応計画/回避・転嫁・軽減・受容、リスク監視）",
            "プロジェクト調達マネジメント（調達計画、RFP/提案依頼書、ベンダー選定基準、契約管理、SLA）",
            "プロジェクトステークホルダーマネジメント（ステークホルダー特定、エンゲージメント管理）",
            "アジャイルプロジェクトマネジメント（スクラム、アジャイルマニフェスト、ベロシティ、バーンダウンチャート）"
          ]},
          {name:"システム企画（レベル3）", children:["システム化計画（全体最適化、費用対効果分析/ROI、投資評価）","要件定義プロセス（業務要件、機能要件、非機能要件の定義）"]},
          {name:"システム開発技術（レベル3）", children:["開発プロセス（共通フレーム/SLCP、ウォーターフォールモデル、プロトタイピングモデル）","設計・テスト（システム設計、単体テスト、結合テスト、システムテスト、運用テスト）"]},
          {name:"ソフトウェア開発管理技術（レベル3）", children:["構成管理・変更管理（構成識別、バージョン管理、ベースライン、構成監査、リポジトリ）"]},
          {name:"サービスマネジメント（レベル3）", children:["サービスマネジメント（ITIL、サービスデザイン、サービス移行、サービス運用、インシデント管理、問題管理、リリース管理）"]},
          {name:"情報セキュリティ（レベル3）", children:["情報セキュリティ管理（ISMS、セキュリティポリシー、リスクアセスメント、組織的・人的セキュリティ対策）","セキュリティ技術（暗号化技術、共通鍵・公開鍵暗号、デジタル署名、認証技術、マルウェア対策、サイバー攻撃手法と対策）"]},
          {name:"法務（レベル3）", children:["知的財産権（著作権法、産業財産権、特許法、不正競争防止法）","労働関連法・取引契約（労働者派遣法、民法/請負契約・準委任契約、下請法、機密保持契約/NDA）"]},
          {name:"プロジェクトの立ち上げ・計画", children:[
            "プロジェクト目標（スコープ・納期・コスト・品質）の明確化と制約条件の評価",
            "開発規模・工数の見積もり（ファンクションポイント法、類推法などの妥当性検証）",
            "要員計画・体制構築（スキルバランス、複数ベンダー混在環境の体制策定）",
            "スケジュール・WBSの策定（クリティカルパスの特定、先行・後行タスクの整合性）"
          ]},
          {name:"プロジェクトの実行・コントロール", children:[
            "進捗・コストの予実管理（EVMを用いたトレンド分析、遅延回復策の策定）",
            "品質管理（バグ密度・テスト消化率の分析、品質目標未達への対策、レビューの形骸化防止）",
            "課題・リスク管理（予期せぬリスクの顕在化、課題の優先順位付けと解決策の実行）",
            "変更管理（顧客からの仕様変更、法改正に伴う追加要件の影響分析と承認プロセス）",
            "ステークホルダー・チームマネジメント（顧客との合意形成、要員のモチベーション維持、多国籍/リモート体制の管理）"
          ]},
          {name:"プロジェクトの終結", children:["成果物の納品と顧客による受け入れテストの支援","プロジェクト全体の振り返りと評価（Lessons Learned/組織の資産化）"]}
        ]}
      ]
    },
    { id:"note", title:"note作成", items:["書籍確認","タスク整理"] },
    { id:"toeic700", title:"TOEIC700点", items:["TOEIC700点"] },
    { id:"diet", title:"ダイエット", items:["体脂肪率25%","体重45kg"] },
    { id:"fukugyo", title:"複業案件", items:["複業クラウド案件調査","案件調査確認スクリプト作成","現在の資格や状況と照会","いつまでに何を取得するのか決定"] },
    { id:"marketing", title:"マーケティング系資格", items:["マーケティング系資格"] },
    { id:"ec", title:"(ECサイト実証実験)", items:["(ECサイト実証実験)"] },
  ];

  const DATA_KEY = "genzaichi_tracker_data_v1";
  const STORE_KEY = "genzaichi_tracker_v5";

  let workingData;
  try{
    const raw = localStorage.getItem(DATA_KEY);
    workingData = raw ? JSON.parse(raw) : JSON.parse(JSON.stringify(SEED));
  }catch(e){ workingData = JSON.parse(JSON.stringify(SEED)); }
  // マイグレーション：カード名の改名を既存保存データにも反映
  (function migrate(){
    const f = workingData.find(c=>c.id==="fukugyo");
    if(f && f.title==="複業クラウド案件獲得"){ f.title = "複業案件"; }
  })();

  let state = {};
  try{ const raw = localStorage.getItem(STORE_KEY); if(raw) state = JSON.parse(raw); }catch(e){ state = {}; }

  function saveData(){ try{ localStorage.setItem(DATA_KEY, JSON.stringify(workingData)); }catch(e){} }
  function saveState(){ try{ localStorage.setItem(STORE_KEY, JSON.stringify(state)); }catch(e){} }

  let editMode = false;
  let openPaths = {}; // 開いているグループのキーを記憶（再描画時も開閉状態を保つ）

  const pctEls = {}, fillEls = {}, linkEls = {};
  function reg(map, key, el){ (map[key] = map[key]||[]).push(el); }

  function computeVal(node, path){
    if(typeof node === "string"){
      return state[path.concat(node).join("|")] || 0;
    }
    const cp = path.concat(node.name);
    const vals = node.children.map(c=>computeVal(c, cp));
    return vals.length ? Math.round(vals.reduce((a,b)=>a+b,0)/vals.length) : 0;
  }

  function renameLeaf(oldKey, newKey){
    if(state[oldKey]!=null){ state[newKey] = state[oldKey]; delete state[oldKey]; }
  }

  function buildLeaf(name, path, siblings, idx){
    const key = path.concat(name).join("|");
    const val = state[key] || 0;
    const row = document.createElement("div");
    row.className = "item-row";
    row.innerHTML = `<div class="item-body">
        <div class="item-top"><span>${name}</span><span class="item-pct">${val}%</span></div>
        <input type="range" min="0" max="100" step="5" value="${val}">
        <div class="tlinktime" style="display:none;"></div>
      </div>`;
    reg(pctEls, key, row.querySelector(".item-pct"));
    reg(linkEls, key, row.querySelector(".tlinktime"));
    row.querySelector("input").addEventListener("input", function(){
      state[key] = parseInt(this.value,10);
      saveState();
      updateAll();
    });
    if(editMode){
      const ebtn = document.createElement("button");
      ebtn.className = "ebtn"; ebtn.textContent = "✏️"; ebtn.title="名前を変更";
      ebtn.addEventListener("click", ()=>{
        const nn = prompt("項目名を編集", name);
        if(nn && nn.trim() && nn.trim()!==name){
          const newKey = path.concat(nn.trim()).join("|");
          renameLeaf(key, newKey);
          siblings[idx] = nn.trim();
          saveData(); saveState(); renderAll();
        }
      });
      const dbtn = document.createElement("button");
      dbtn.className = "ebtn del"; dbtn.textContent = "🗑"; dbtn.title="削除";
      dbtn.addEventListener("click", ()=>{
        if(confirm(`「${name}」を削除しますか？`)){
          siblings.splice(idx,1);
          delete state[key];
          saveData(); saveState(); renderAll();
        }
      });
      row.appendChild(ebtn); row.appendChild(dbtn);
    }
    return row;
  }

  function buildAddRow(children, path, groupKey){
    const wrap = document.createElement("div");
    wrap.className = "addrow";
    const b1 = document.createElement("button");
    b1.className = "addbtn"; b1.textContent = "＋ 小項目";
    b1.addEventListener("click", ()=>{
      const nn = prompt("小項目名を入力");
      if(nn && nn.trim()){ children.push(nn.trim()); saveData(); openPaths[groupKey]=true; renderAll(); }
    });
    const b2 = document.createElement("button");
    b2.className = "addbtn"; b2.textContent = "＋ グループ";
    b2.addEventListener("click", ()=>{
      const nn = prompt("グループ名を入力");
      if(nn && nn.trim()){ children.push({name:nn.trim(), children:[]}); saveData(); openPaths[groupKey]=true; renderAll(); }
    });
    wrap.appendChild(b1); wrap.appendChild(b2);
    return wrap;
  }

  function buildGroup(node, path, depth, siblings, idx){
    const cp = path.concat(node.name);
    const key = cp.join("|");
    const val = computeVal(node, path);
    const wrap = document.createElement("div");
    wrap.className = "group depth"+depth + (openPaths[key] ? " open" : "");
    const head = document.createElement("div");
    head.className = "ghead";
    head.innerHTML = `<span class="gname"><span class="arrow">▸</span><span>${node.name}</span></span><span class="gpct">${val}%</span>`;
    head.addEventListener("click", (e)=>{
      if(e.target.classList.contains("ebtn")) return;
      const isOpen = wrap.classList.toggle("open");
      if(isOpen) openPaths[key]=true; else delete openPaths[key];
    });
    if(editMode){
      const ebtn = document.createElement("button");
      ebtn.className = "ebtn"; ebtn.textContent = "✏️"; ebtn.title="名前を変更";
      ebtn.addEventListener("click", (e)=>{
        e.stopPropagation();
        const nn = prompt("グループ名を編集", node.name);
        if(nn && nn.trim() && nn.trim()!==node.name){ node.name = nn.trim(); saveData(); renderAll(); }
      });
      const dbtn = document.createElement("button");
      dbtn.className = "ebtn del"; dbtn.textContent = "🗑"; dbtn.title="削除";
      dbtn.addEventListener("click", (e)=>{
        e.stopPropagation();
        if(confirm(`「${node.name}」とその中身をすべて削除しますか？`)){
          siblings.splice(idx,1);
          saveData(); renderAll();
        }
      });
      head.appendChild(ebtn); head.appendChild(dbtn);
    }
    const track = document.createElement("div");
    track.className = "gtrack";
    track.innerHTML = `<div class="gfill" style="width:${val}%"></div>`;
    const childrenEl = document.createElement("div");
    childrenEl.className = "gchildren";
    node.children.forEach((c,i)=>{
      childrenEl.appendChild(typeof c === "string" ? buildLeaf(c, cp, node.children, i) : buildGroup(c, cp, depth+1, node.children, i));
    });
    if(editMode) childrenEl.appendChild(buildAddRow(node.children, cp, key));
    wrap.appendChild(head);
    wrap.appendChild(track);
    wrap.appendChild(childrenEl);
    reg(pctEls, key, head.querySelector(".gpct"));
    reg(fillEls, key, track.querySelector(".gfill"));
    return wrap;
  }

  function cardVal(card){
    const path = [card.id];
    const vals = card.items.map(it => computeVal(it, path));
    return vals.length ? Math.round(vals.reduce((a,b)=>a+b,0)/vals.length) : 0;
  }

  const sectionsEl = document.getElementById("sections");

  function buildCard(card, idx){
    const path = [card.id];
    const val = cardVal(card);
    const wrap = document.createElement("div");
    wrap.className = "card";
    const chead = document.createElement("div");
    chead.className = "chead";
    chead.innerHTML = `<h2>${card.title}</h2><span class="pct">${val}%</span>`;
    if(editMode){
      const ebtn = document.createElement("button");
      ebtn.className = "ebtn"; ebtn.textContent = "✏️"; ebtn.title="名前を変更";
      ebtn.addEventListener("click", ()=>{
        const nn = prompt("カード名を編集", card.title);
        if(nn && nn.trim() && nn.trim()!==card.title){ card.title = nn.trim(); saveData(); renderAll(); }
      });
      const dbtn = document.createElement("button");
      dbtn.className = "ebtn del"; dbtn.textContent = "🗑"; dbtn.title="削除";
      dbtn.addEventListener("click", ()=>{
        if(confirm(`「${card.title}」カードを削除しますか？`)){
          workingData.splice(idx,1);
          saveData(); renderAll();
        }
      });
      chead.appendChild(ebtn); chead.appendChild(dbtn);
    }
    wrap.appendChild(chead);
    const track = document.createElement("div");
    track.className = "track";
    track.innerHTML = `<div class="fill" style="width:${val}%"></div>`;
    wrap.appendChild(track);
    const cardLink = document.createElement("div");
    cardLink.className = "tlinktime"; cardLink.style.display = "none";
    wrap.appendChild(cardLink);
    reg(linkEls, card.id, cardLink);
    reg(pctEls, card.id, chead.querySelector(".pct"));
    reg(fillEls, card.id, track.querySelector(".fill"));
    card.items.forEach((it,i)=>{
      wrap.appendChild(typeof it === "string" ? buildLeaf(it, path, card.items, i) : buildGroup(it, path, 1, card.items, i));
    });
    if(editMode) wrap.appendChild(buildAddRow(card.items, path, card.id));
    sectionsEl.appendChild(wrap);
  }

  function collectLeafVals(items, path, out){
    items.forEach(it=>{
      if(typeof it === "string"){ out.push(state[path.concat(it).join("|")]||0); }
      else { collectLeafVals(it.children, path.concat(it.name), out); }
    });
  }
  function updateGroups(items, path){
    items.forEach(it=>{
      if(typeof it !== "string"){
        const cp = path.concat(it.name);
        const key = cp.join("|");
        const v = computeVal(it, path);
        (pctEls[key]||[]).forEach(el=>el.textContent=v+"%");
        (fillEls[key]||[]).forEach(el=>el.style.width=v+"%");
        updateGroups(it.children, cp);
      }
    });
  }
  function updateAll(){
    let all = [];
    workingData.forEach(card=>{
      const cv = cardVal(card);
      (pctEls[card.id]||[]).forEach(el=>el.textContent=cv+"%");
      (fillEls[card.id]||[]).forEach(el=>el.style.width=cv+"%");
      collectLeafVals(card.items, [card.id], all);
      updateGroups(card.items, [card.id]);
    });
    const overall = all.length ? Math.round(all.reduce((a,b)=>a+b,0)/all.length) : 0;
    document.getElementById("overallPct").textContent = overall+"%";
    document.getElementById("overallFill").style.width = overall+"%";
    if(window.refreshGoalProgress) window.refreshGoalProgress();
  }

  function renderAll(){
    sectionsEl.innerHTML = "";
    Object.keys(pctEls).forEach(k=>delete pctEls[k]);
    Object.keys(fillEls).forEach(k=>delete fillEls[k]);
    Object.keys(linkEls).forEach(k=>delete linkEls[k]);
    workingData.forEach((c,i)=>buildCard(c,i));
    updateAll();
    document.getElementById("addCardBtn").style.display = editMode ? "block" : "none";
    document.getElementById("editToggle").classList.toggle("on", editMode);
    refreshLinkedTimes();
  }

  // フランクリンタブと連携：紐付けられたタスクの実績時間を表示（スライダー・％には影響しない）
  function refreshLinkedTimes(){
    if(typeof window.getLinkedMinutes !== "function") return;
    Object.keys(linkEls).forEach(key=>{
      const mins = window.getLinkedMinutes(key) || 0;
      const hrs = Math.round(mins/6)/10; // 0.1時間単位
      linkEls[key].forEach(el=>{
        if(mins>0){ el.style.display="block"; el.textContent = "⏱ "+hrs+"時間"; }
        else{ el.style.display="none"; }
      });
    });
  }
  window.refreshTrackerLinkedTimes = refreshLinkedTimes;

  window.getTrackerLeafOptions = function(){
    const out = [];
    function walk(items, path, labelPath){
      items.forEach(it=>{
        if(typeof it === "string"){
          out.push({ key: path.concat(it).join("|"), label: labelPath.concat(it).join(" > ") });
        } else {
          walk(it.children, path.concat(it.name), labelPath.concat(it.name));
        }
      });
    }
    workingData.forEach(card=> walk(card.items, [card.id], [card.title]));
    return out;
  };

  // Annual goalの紐付け用：カード／グループ／小項目すべてを選択肢にする
  window.getTrackerNodeOptions = function(){
    const out = [];
    function walk(items, path, labelPath){
      items.forEach(it=>{
        if(typeof it === "string"){
          out.push({ key: path.concat(it).join("|"), label: labelPath.concat(it).join(" > ") });
        } else {
          out.push({ key: path.concat(it.name).join("|"), label: labelPath.concat(it.name).join(" > ") });
          walk(it.children, path.concat(it.name), labelPath.concat(it.name));
        }
      });
    }
    workingData.forEach(card=>{
      out.push({ key: card.id, label: card.title });
      walk(card.items, [card.id], [card.title]);
    });
    return out;
  };

  // 指定ノード（カード／グループ／小項目）の進捗％を取得
  window.getTrackerNodeValue = function(key){
    const parts = key.split("|");
    const card = workingData.find(c=>c.id===parts[0]);
    if(!card) return 0;
    if(parts.length===1) return cardVal(card);
    let items = card.items, path = [parts[0]];
    for(let i=1;i<parts.length;i++){
      const name = parts[i];
      const node = items.find(it => typeof it==="string" ? it===name : it.name===name);
      if(!node) return 0;
      if(i === parts.length-1){
        return typeof node === "string" ? (state[path.concat(name).join("|")]||0) : computeVal(node, path);
      }
      if(typeof node === "string") return 0;
      path = path.concat(node.name);
      items = node.children;
    }
    return 0;
  };

  document.getElementById("editToggle").addEventListener("click", ()=>{
    editMode = !editMode;
    renderAll();
  });
  document.getElementById("addCardBtn").addEventListener("click", ()=>{
    const nn = prompt("新しいカード名を入力");
    if(nn && nn.trim()){
      const id = "c" + Date.now();
      workingData.push({id, title:nn.trim(), items:[]});
      saveData(); renderAll();
    }
  });
  document.getElementById("resetBtn").addEventListener("click", ()=>{
    if(confirm("すべての内容と進捗を初期状態にリセットしますか？")){
      state = {}; workingData = JSON.parse(JSON.stringify(SEED));
      saveState(); saveData(); location.reload();
    }
  });

  renderAll();
})();
</script>

<script>
(function(){
  // Annual goal：目標（タイトルは定型リストから選択・追加可／期限／達成率）の管理
  const GOALS_KEY = "annual_goals_v1";
  let goals = [];
  try{ const raw = localStorage.getItem(GOALS_KEY); if(raw) goals = JSON.parse(raw); }catch(e){ goals = []; }
  // マイグレーション：旧フィールド名linkedCardIdをlinkedNodeKeyへ
  goals.forEach(g=>{ if(g.linkedCardId && !g.linkedNodeKey){ g.linkedNodeKey = g.linkedCardId; } });
  function gSave(){ try{ localStorage.setItem(GOALS_KEY, JSON.stringify(goals)); }catch(e){} }
  window.getAnnualGoals = function(){ return goals; };
  window.getAnnualGoalProgress = function(id){
    const g = goals.find(x=>x.id===id);
    if(!g || !g.linkedNodeKey) return 0;
    return (typeof window.getTrackerNodeValue==="function") ? window.getTrackerNodeValue(g.linkedNodeKey) : 0;
  };

  // タイトル→トラッカーのノードキーを自動対応
  const TITLE_MAP = [
    {title:"基本情報技術者試験", key:"it|基本情報技術者試験"},
    {title:"G検定", key:"it|G検定"},
    {title:"SQL", key:"it|SQL"},
    {title:"PM系資格", key:"it|PM系資格"},
    {title:"note作成", key:"note"},
    {title:"TOEIC", key:"toeic700"},
    {title:"ダイエット", key:"diet"},
    {title:"複業案件", key:"fukugyo"},
    {title:"マーケティング系資格", key:"marketing"},
    {title:"(ECサイト実証実験)", key:"ec"}
  ];
  const CUSTOM_TITLES_KEY = "annual_goal_custom_titles_v1";
  let customTitles = [];
  try{ const raw = localStorage.getItem(CUSTOM_TITLES_KEY); if(raw) customTitles = JSON.parse(raw); }catch(e){ customTitles = []; }
  function ctSave(){ try{ localStorage.setItem(CUSTOM_TITLES_KEY, JSON.stringify(customTitles)); }catch(e){} }
  function keyForTitle(title){ const f = TITLE_MAP.find(o=>o.title===title); return f ? f.key : null; }

  const titleSel = document.getElementById("goalTitle");
  function populateTitleSel(){
    const cur = titleSel.value;
    const all = TITLE_MAP.map(o=>o.title).concat(customTitles);
    titleSel.innerHTML = all.map(t=>`<option value="${t}">${t}</option>`).join("") + '<option value="__add__">＋ 新しいタイトルを追加</option>';
    if(all.includes(cur)) titleSel.value = cur;
  }
  titleSel.addEventListener("change", ()=>{
    if(titleSel.value === "__add__"){
      const nn = prompt("新しい目標タイトルを入力");
      populateTitleSel();
      if(nn && nn.trim()){
        customTitles.push(nn.trim()); ctSave(); populateTitleSel();
        titleSel.value = nn.trim();
      }
    }
  });
  populateTitleSel();

  function nodeLabel(key){
    const opts = (typeof window.getTrackerNodeOptions === "function") ? window.getTrackerNodeOptions() : [];
    const f = opts.find(o=>o.key===key);
    return f ? f.label : null;
  }
  function fmtDate(d){ if(!d) return "期限未設定"; const [y,m,dd]=d.split("-"); return `${y}/${m}/${dd}`; }

  function renderGoals(){
    populateTitleSel();
    const listEl = document.getElementById("goalList");
    listEl.innerHTML = "";
    if(goals.length===0){
      listEl.innerHTML = '<div class="noTask">まだ目標がありません</div>';
      return;
    }
    goals.forEach((g,i)=>{
      const pct = window.getAnnualGoalProgress(g.id);
      const label = nodeLabel(g.linkedNodeKey);
      const row = document.createElement("div");
      row.className = "goalrow";
      row.style.flexDirection = "column";
      row.style.alignItems = "stretch";
      row.innerHTML = `<div style="display:flex; align-items:center; gap:10px;">
          <span class="gtitle">${g.title}</span>
          ${label ? `<span class="catbadge">${label}</span>` : ""}
          <span class="gdeadline">${fmtDate(g.deadline)}</span>
          <span class="gdeadline">${pct}%</span>
          <button class="ebtn del">🗑</button>
        </div>
        <div class="gtrack"><div class="gfill" style="width:${pct}%"></div></div>`;
      row.querySelector(".del").addEventListener("click", ()=>{
        if(confirm(`「${g.title}」を削除しますか？`)){ goals.splice(i,1); gSave(); renderGoals(); }
      });
      listEl.appendChild(row);
    });
  }
  window.refreshGoalProgress = renderGoals;

  document.getElementById("addGoalBtn").addEventListener("click", ()=>{
    const title = titleSel.value;
    if(!title || title==="__add__"){ alert("目標タイトルを選択してください"); return; }
    const deadline = document.getElementById("goalDeadline").value;
    const linkedNodeKey = keyForTitle(title);
    goals.push({id:"g"+Date.now(), title, deadline, linkedNodeKey});
    gSave();
    document.getElementById("goalDeadline").value = "";
    renderGoals();
  });

  renderGoals();
})();
</script>

<script>
(function(){
  // タブ切り替え
  document.querySelectorAll(".tabbtn").forEach(btn=>{
    btn.addEventListener("click", ()=>{
      document.querySelectorAll(".tabbtn").forEach(b=>b.classList.remove("active"));
      document.querySelectorAll(".tabpanel").forEach(p=>p.classList.remove("active"));
      btn.classList.add("active");
      document.getElementById("tab-"+btn.dataset.tab).classList.add("active");
      if(btn.dataset.tab==="franklin") populateLinkOptions();
      if(btn.dataset.tab==="monthly") renderMonthly();
    });
  });

  // フランクリン：デイリータスク管理
  const F_KEY = "franklin_tasks_v1";
  let fData = {};
  try{ const raw = localStorage.getItem(F_KEY); if(raw) fData = JSON.parse(raw); }catch(e){ fData = {}; }
  function fSave(){ try{ localStorage.setItem(F_KEY, JSON.stringify(fData)); }catch(e){} }

  let curDate = new Date();
  function dstr(d){ return d.getFullYear()+"-"+String(d.getMonth()+1).padStart(2,"0")+"-"+String(d.getDate()).padStart(2,"0"); }
  function dlabel(d){
    const w = ["日","月","火","水","木","金","土"][d.getDay()];
    return (d.getMonth()+1)+"月"+d.getDate()+"日（"+w+"）";
  }
  function tasksFor(key){ if(!fData[key]) fData[key]=[]; return fData[key]; }

  // 現在地トラッカーとの連携：紐付けキーごとの実績時間（分）を合計
  function getLinkedMinutesFor(key){
    let sum = 0;
    Object.keys(fData).forEach(d=>{
      fData[d].forEach(t=>{ if(t.linkKey===key) sum += (t.actual||0); });
    });
    return sum;
  }
  window.getLinkedMinutes = getLinkedMinutesFor;

  // Monthly目標（Annual goalと紐付け・月ごとに管理）
  const MGOALS_KEY = "monthly_goal_items_v1";
  let mGoals = {};
  try{ const raw = localStorage.getItem(MGOALS_KEY); if(raw) mGoals = JSON.parse(raw); }catch(e){ mGoals = {}; }
  function mgSave(){ try{ localStorage.setItem(MGOALS_KEY, JSON.stringify(mGoals)); }catch(e){} }
  function monthKeyOf(d){ return d.getFullYear()+"-"+String(d.getMonth()+1).padStart(2,"0"); }
  function monthlyItemsFor(d){ const k=monthKeyOf(d); if(!mGoals[k]) mGoals[k]=[]; return mGoals[k]; }

  // Dailyタスクの紐付け先：閲覧中の日付が属する月のMonthly目標
  function populateLinkOptions(){
    const sel = document.getElementById("taskLink");
    const cur = sel.value;
    const items = monthlyItemsFor(curDate);
    sel.innerHTML = '<option value="">Monthlyの目標と紐付け（任意）</option>' +
      items.map(o=>`<option value="${o.id}">${o.title}</option>`).join("");
    sel.value = cur;
  }

  const groupsEl = document.getElementById("taskGroups");
  function linkLabelOf(key){
    let found = null;
    Object.keys(mGoals).forEach(mk=>{ const f = mGoals[mk].find(o=>o.id===key); if(f) found = f; });
    return found ? found.title : "(削除済み項目)";
  }
  function gcalUrl(text, dateKey, details){
    const start = dateKey.replace(/-/g,"");
    const nd = new Date(dateKey); nd.setDate(nd.getDate()+1);
    const end = dstr(nd).replace(/-/g,"");
    return "https://calendar.google.com/calendar/render?action=TEMPLATE&text="+encodeURIComponent(text)+"&dates="+start+"/"+end+"&details="+encodeURIComponent(details||"");
  }
  function icsFor(text, dateKey, details){
    const start = dateKey.replace(/-/g,"");
    const nd = new Date(dateKey); nd.setDate(nd.getDate()+1);
    const end = dstr(nd).replace(/-/g,"");
    return "BEGIN:VCALENDAR\r\nVERSION:2.0\r\nBEGIN:VEVENT\r\nDTSTART;VALUE=DATE:"+start+"\r\nDTEND;VALUE=DATE:"+end+"\r\nSUMMARY:"+text+"\r\nDESCRIPTION:"+(details||"")+"\r\nEND:VEVENT\r\nEND:VCALENDAR\r\n";
  }
  async function saveIcs(text, dateKey, details){
    try{
      const downloads = await claude.use("downloads");
      if(!downloads){ alert("この環境ではカレンダー保存機能が使えません。📅ボタン（Googleカレンダー）をお使いください。"); return; }
      await downloads.save({ filename: text.replace(/[\\/:*?"<>|]/g,"") + ".ics", data: icsFor(text, dateKey, details) });
    }catch(e){ alert("保存できませんでした。"); }
  }
  const PORDER = ["A","B","C"];
  const PLABEL = {A:"A：重要・優先度高", B:"B：普通", C:"C：余裕があれば"};

  function renderFranklin(){
    populateLinkOptions();
    const key = dstr(curDate);
    document.getElementById("dayLabel").textContent = dlabel(curDate);
    const tasks = tasksFor(key);

    groupsEl.innerHTML = "";
    let totalEst=0, totalAct=0, doneCount=0;

    PORDER.forEach(p=>{
      const list = tasks.filter(t=>t.priority===p);
      const g = document.createElement("div");
      g.className = "pgroup";
      g.innerHTML = `<h3><span class="pbadge ${p}">${p}</span>${PLABEL[p]}</h3>`;
      if(list.length===0){
        const none = document.createElement("div");
        none.className = "noTask";
        none.textContent = "登録なし";
        g.appendChild(none);
      }
      list.forEach(t=>{
        totalEst += (t.est||0); totalAct += (t.actual||0);
        if(t.done) doneCount++;
        const row = document.createElement("div");
        row.className = "task" + (t.done ? " done" : "");
        row.innerHTML = `<input type="checkbox" ${t.done?"checked":""}>
          <div class="tbody">
            <div class="ttext">${t.text}</div>
            <div class="tmeta">
              <span>予定 <input type="number" min="0" step="5" value="${t.est||0}" class="estIn"> 分</span>
              <span>実績 <input type="number" min="0" step="5" value="${t.actual||0}" class="actIn"> 分</span>
              ${t.linkKey ? `<span>🔗 ${linkLabelOf(t.linkKey)}</span>` : ""}
              <a class="ebtn gcalLink" href="${gcalUrl(t.text, key, '優先度'+t.priority+'／予定'+(t.est||0)+'分')}" target="_blank" rel="noopener" title="Googleカレンダーに追加">📅</a>
              <button class="ebtn icsBtn" title="Appleカレンダー/リマインダーに追加(.ics)">🍎</button>
              <button class="ebtn del">🗑</button>
            </div>
          </div>`;
        row.querySelector(".icsBtn").addEventListener("click", ()=>{
          saveIcs(t.text, key, '優先度'+t.priority+'／予定'+(t.est||0)+'分');
        });
        row.querySelector('input[type=checkbox]').addEventListener("change", function(){
          t.done = this.checked; fSave(); renderFranklin();
        });
        row.querySelector(".estIn").addEventListener("change", function(){
          t.est = parseInt(this.value,10)||0; fSave(); renderFranklin();
        });
        row.querySelector(".actIn").addEventListener("change", function(){
          t.actual = parseInt(this.value,10)||0; fSave(); renderFranklin();
          if(window.refreshTrackerLinkedTimes) window.refreshTrackerLinkedTimes();
        });
        row.querySelector(".del").addEventListener("click", ()=>{
          const i = tasks.indexOf(t);
          if(i>-1){ tasks.splice(i,1); fSave(); renderFranklin(); }
          if(window.refreshTrackerLinkedTimes) window.refreshTrackerLinkedTimes();
        });
        g.appendChild(row);
      });
      groupsEl.appendChild(g);
    });

    const pct = tasks.length ? Math.round(doneCount/tasks.length*100) : 0;
    document.getElementById("fPct").textContent = pct+"%";
    document.getElementById("fFill").style.width = pct+"%";
    document.getElementById("fTime").textContent = `予定 ${totalEst}分 ／ 実績 ${totalAct}分`;
  }

  document.getElementById("addTaskBtn").addEventListener("click", ()=>{
    const textEl = document.getElementById("taskText");
    const text = textEl.value.trim();
    if(!text) return;
    const priority = document.getElementById("taskPriority").value;
    const est = parseInt(document.getElementById("taskEst").value,10) || 0;
    const linkKey = document.getElementById("taskLink").value || null;
    tasksFor(dstr(curDate)).push({text, priority, est, actual:0, done:false, linkKey});
    fSave();
    textEl.value = "";
    document.getElementById("taskEst").value = "";
    renderFranklin();
    if(window.refreshTrackerLinkedTimes) window.refreshTrackerLinkedTimes();
  });

  // よく使うタスク（テンプレート）
  const TPL_KEY = "franklin_templates_v1";
  let templates = [];
  try{ const raw = localStorage.getItem(TPL_KEY); if(raw) templates = JSON.parse(raw); }catch(e){ templates = []; }
  function tplSave(){ try{ localStorage.setItem(TPL_KEY, JSON.stringify(templates)); }catch(e){} }

  function renderTemplates(){
    const el = document.getElementById("templateChips");
    el.innerHTML = "";
    if(templates.length===0){
      el.innerHTML = '<span class="noTask">まだありません。フォーム入力後「☆ よく使うタスクに登録」で追加できます</span>';
      return;
    }
    templates.forEach((tpl, i)=>{
      const chip = document.createElement("div");
      chip.className = "chip";
      chip.innerHTML = `<span>${chip_icon(tpl.priority)} ${tpl.text}</span><span class="cx">✕</span>`;
      chip.querySelector("span:first-child").addEventListener("click", ()=>{
        tasksFor(dstr(curDate)).push({text:tpl.text, priority:tpl.priority, est:tpl.est, actual:0, done:false, linkKey:tpl.linkKey||null});
        fSave(); renderFranklin();
        if(window.refreshTrackerLinkedTimes) window.refreshTrackerLinkedTimes();
      });
      chip.querySelector(".cx").addEventListener("click", (e)=>{
        e.stopPropagation();
        templates.splice(i,1); tplSave(); renderTemplates();
      });
      el.appendChild(chip);
    });
  }
  function chip_icon(p){ return p==="A"?"🔴":p==="B"?"🟠":"🟢"; }

  document.getElementById("saveTemplateBtn").addEventListener("click", ()=>{
    const text = document.getElementById("taskText").value.trim();
    if(!text){ alert("タスク内容を入力してから登録してください"); return; }
    const priority = document.getElementById("taskPriority").value;
    const est = parseInt(document.getElementById("taskEst").value,10) || 0;
    const linkKey = document.getElementById("taskLink").value || null;
    templates.push({text, priority, est, linkKey});
    tplSave(); renderTemplates();
  });
  renderTemplates();

  document.getElementById("prevDay").addEventListener("click", ()=>{ curDate.setDate(curDate.getDate()-1); renderFranklin(); });
  document.getElementById("nextDay").addEventListener("click", ()=>{ curDate.setDate(curDate.getDate()+1); renderFranklin(); });
  document.getElementById("todayBtn").addEventListener("click", ()=>{ curDate = new Date(); renderFranklin(); });

  // マンスリー表示
  let curMonth = new Date();
  const monthListEl = document.getElementById("monthList");

  // 振り返りの保存（月ごと）
  const MNOTE_KEY = "monthly_notes_v1";
  let mNotes = {};
  try{ const raw = localStorage.getItem(MNOTE_KEY); if(raw) mNotes = JSON.parse(raw); }catch(e){ mNotes = {}; }
  function mSave(){ try{ localStorage.setItem(MNOTE_KEY, JSON.stringify(mNotes)); }catch(e){} }
  function noteFor(){ const k=monthKeyOf(curMonth); if(!mNotes[k]) mNotes[k]={reflection:""}; return mNotes[k]; }

  function populateMGoalLinkSel(){
    const sel = document.getElementById("mgLinkGoal");
    const cur = sel.value;
    const ag = (typeof window.getAnnualGoals === "function") ? window.getAnnualGoals() : [];
    sel.innerHTML = '<option value="">Annual goalと紐付け（任意）</option>' +
      ag.map(g=>`<option value="${g.id}">${g.title}</option>`).join("");
    sel.value = cur;
  }

  function annualGoalTitle(id){
    const ag = (typeof window.getAnnualGoals === "function") ? window.getAnnualGoals() : [];
    const f = ag.find(g=>g.id===id);
    return f ? f.title : null;
  }

  function renderMonthGoals(){
    populateMGoalLinkSel();
    const el = document.getElementById("monthGoalTargets");
    const items = monthlyItemsFor(curMonth);
    el.innerHTML = "";
    if(items.length===0){
      el.innerHTML = '<div class="noTask">今月の目標を登録しましょう</div>';
    } else {
      items.forEach((it,i)=>{
        const mins = getLinkedMinutesFor(it.id);
        const hrs = Math.round(mins/6)/10;
        const row = document.createElement("div");
        row.className = "mgoalrow";
        const agTitle = annualGoalTitle(it.linkedGoalId);
        row.innerHTML = `<div class="gname">${it.title} ${agTitle?`<span class="catbadge">${agTitle}</span>`:""}</div>
          <div class="tmeta">目標 ${it.targetPct||0}% ／ 実績時間 ${hrs}時間</div>
          <div class="item-top"><span>達成率</span><span class="item-pct">${it.progressPct||0}%</span></div>
          <input type="range" min="0" max="100" step="5" value="${it.progressPct||0}" class="mgRange">
          <button class="ebtn del" style="float:right;">🗑 削除</button>`;
        row.querySelector(".mgRange").addEventListener("input", function(){
          it.progressPct = parseInt(this.value,10);
          row.querySelector(".item-pct").textContent = it.progressPct+"%";
          mgSave();
        });
        row.querySelector(".del").addEventListener("click", ()=>{
          if(confirm(`「${it.title}」を削除しますか？`)){ items.splice(i,1); mgSave(); renderMonthGoals(); }
        });
        el.appendChild(row);
      });
    }
    const refEl = document.getElementById("monthReflect");
    refEl.value = noteFor().reflection || "";
  }
  document.getElementById("addMGoalBtn").addEventListener("click", ()=>{
    const titleEl = document.getElementById("mgTitle");
    const title = titleEl.value.trim();
    if(!title){ alert("目標を入力してください"); return; }
    const linkedGoalId = document.getElementById("mgLinkGoal").value || null;
    const targetPct = parseInt(document.getElementById("mgTarget").value,10) || 0;
    monthlyItemsFor(curMonth).push({id:"mg"+Date.now(), title, linkedGoalId, targetPct, progressPct:0});
    mgSave();
    titleEl.value = "";
    document.getElementById("mgTarget").value = "";
    renderMonthGoals();
  });
  document.getElementById("monthReflect").addEventListener("change", function(){
    noteFor().reflection = this.value; mSave();
  });

  function renderMonthly(){
    renderMonthGoals();
    document.getElementById("monthLabel").textContent = curMonth.getFullYear()+"年"+(curMonth.getMonth()+1)+"月";
    const y = curMonth.getFullYear(), m = curMonth.getMonth();
    const daysInMonth = new Date(y, m+1, 0).getDate();
    monthListEl.innerHTML = "";
    let monthMin=0, monthTasks=0, monthDone=0, dayCount=0, pctSum=0;

    for(let d=1; d<=daysInMonth; d++){
      const dt = new Date(y, m, d);
      const key = dstr(dt);
      const tasks = fData[key];
      if(!tasks || tasks.length===0) continue;
      dayCount++;
      const done = tasks.filter(t=>t.done).length;
      const act = tasks.reduce((s,t)=>s+(t.actual||0),0);
      monthMin += act; monthTasks += tasks.length; monthDone += done;
      const pct = Math.round(done/tasks.length*100);
      pctSum += pct;
      const w = ["日","月","火","水","木","金","土"][dt.getDay()];
      const row = document.createElement("div");
      row.className = "mrow";
      row.innerHTML = `<div class="mdate">${m+1}/${d}（${w}）</div>
        <div class="mstat">${done}/${tasks.length}件 完了
          <div class="mbar"><div style="width:${pct}%"></div></div>
        </div>
        <div class="mmin">${Math.round(act/6)/10}時間</div>`;
      monthListEl.appendChild(row);
    }
    if(dayCount===0){
      monthListEl.innerHTML = '<div class="noTask">この月の記録はまだありません</div>';
    }
    document.getElementById("mTime").textContent = (Math.round(monthMin/6)/10)+"時間";
    const avgPct = dayCount ? Math.round(pctSum/dayCount) : 0;
    document.getElementById("mSummary").textContent = `タスク ${monthTasks}件／完了 ${monthDone}件（平均達成率 ${avgPct}%）`;
  }

  document.getElementById("prevMonth").addEventListener("click", ()=>{ curMonth.setMonth(curMonth.getMonth()-1); renderMonthly(); });
  document.getElementById("nextMonth").addEventListener("click", ()=>{ curMonth.setMonth(curMonth.getMonth()+1); renderMonthly(); });
  document.getElementById("thisMonthBtn").addEventListener("click", ()=>{ curMonth = new Date(); renderMonthly(); });

  renderFranklin();
  renderMonthly();
  if(window.refreshTrackerLinkedTimes) window.refreshTrackerLinkedTimes();
})();
</script>
</body>
</html>
