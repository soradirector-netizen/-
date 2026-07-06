<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>インターネット概論</title>

<style>

*{
    margin:0;
    padding:0;
    box-sizing:border-box;
    font-family:"Yu Gothic","Meiryo",sans-serif;
}

body{
    background:#f4f7fb;
    padding:40px;
}

.container{

    max-width:1100px;
    margin:auto;

}

h1{

    text-align:center;
    margin-bottom:10px;
    color:#222;

}

.sub{

    text-align:center;
    color:#666;
    margin-bottom:30px;

}

.card{

    background:white;
    border-radius:12px;
    padding:30px;
    box-shadow:0 0 15px rgba(0,0,0,.08);

}

#questionTitle{

    font-size:24px;
    margin-bottom:20px;

}

#questionText{

    white-space:pre-wrap;
    line-height:1.8;
    font-size:18px;
    margin-bottom:25px;

}

textarea{

    width:100%;
    min-height:250px;
    resize:vertical;
    padding:15px;
    font-size:17px;
    line-height:1.8;

}

.buttons{

    margin-top:25px;
    display:flex;
    gap:15px;
    flex-wrap:wrap;

}

button{

    padding:12px 22px;
    border:none;
    cursor:pointer;
    border-radius:8px;
    font-size:16px;
    color:white;

}

#checkBtn{

    background:#1976d2;

}

#nextBtn{

    background:#2e7d32;

}

#answerBtn{

    background:#5e35b1;

}

#shuffleBtn{

    background:#ef6c00;

}

.result{

    margin-top:25px;
    display:none;
    background:#f7f7f7;
    padding:20px;
    border-radius:10px;

}

.result h2{

    margin-bottom:15px;

}

.score{

    font-size:24px;
    color:#d32f2f;
    margin-bottom:15px;

}

#advice{

    margin-top:15px;
    white-space:pre-wrap;
    line-height:1.8;

}

#modelAnswer{

    display:none;
    margin-top:20px;
    background:#eef5ff;
    padding:20px;
    border-radius:10px;
    white-space:pre-wrap;
    line-height:1.8;

}

#progress{

    margin-bottom:25px;
    font-size:18px;
    font-weight:bold;

}

#finalResult{

    display:none;
    margin-top:30px;
    background:#fff7da;
    border-radius:10px;
    padding:25px;

}

table{

    width:100%;
    border-collapse:collapse;
    margin-top:20px;

}

table th,
table td{

    border:1px solid #ccc;
    padding:10px;
    text-align:center;

}

.good{

    color:green;
    font-weight:bold;

}

.bad{

    color:red;
    font-weight:bold;

}

</style>
</head>

<body>

<div class="container">

<h1>ネットワーク テスト採点システム</h1>

<p class="sub">
記述後に採点・アドバイスを表示します。
</p>

<div class="card">

<div id="progress"></div>

<div id="questionTitle"></div>

<div id="questionText"></div>

<textarea id="answer" placeholder="ここに記述してください"></textarea>

<div class="buttons">

<button id="checkBtn">
採点する
</button>

<button id="answerBtn">
解答例を見る
</button>

<button id="nextBtn">
次の問題
</button>

<button id="shuffleBtn">
問題をシャッフル
</button>

</div>

<div class="result" id="result">

<h2>採点結果</h2>

<div class="score" id="score"></div>

<div id="advice"></div>

</div>

<div id="modelAnswer"></div>

<div id="finalResult">

<h2>総合結果</h2>

<h1 id="totalScore"></h1>

<table>

<thead>

<tr>

<th>問題</th>

<th>点数</th>

</tr>

</thead>

<tbody id="scoreTable">

</tbody>

</table>

</div>

</div>

<script>

const questions = [

{
number:1,

question:`通信で利用される基本的な概念としてプロトコルがある。これについて説明せよ。`,

model:`プロトコルとは、コンピュータ同士が正しく通信するために定められた通信手順や規則のことである。代表例としてTCP/IPがある。プロトコルには一体型と分離型があり、一体型は同じメーカーがシステム全体を管理するため安定性が高い反面、変更や拡張がしにくい。一方、分離型は異なるメーカー間でも利用できるため柔軟性や拡張性に優れるが、互換性の問題が生じることもある。インターネットでは分離型プロトコルが採用されている。`,

keywords:[
"プロトコル",
"通信",
"規則",
"TCP/IP",
"一体型",
"分離型",
"柔軟",
"インターネット"
]

},

{
number:2,

question:`プロトコルはOSI参照モデルなどに代表されるように、上下関係がある。これを階層化と呼ぶが、階層化することによってどのような利点があるか説明せよ。`,

model:`OSI参照モデルでは通信機能を複数の階層に分けて管理する。このように階層化することで、各階層の役割が明確になり、設計や開発が容易になる。また、障害が発生した場合でも、どの階層に原因があるかを特定しやすいため、トラブルシューティングや保守・修理を効率的に行える。さらに、一部の階層だけを変更・更新できるため、システム全体への影響を抑えながら改良を進められる。`,

keywords:[
"OSI",
"階層",
"役割",
"保守",
"障害",
"設計",
"開発",
"変更"
]

},

{
number:3,

question:`データリンク層のアクセス制御方式について、CSMA/CD方式とトークンパッシング方式の比較を交えて説明せよ。`,

model:`アクセス制御方式とは、複数の機器が同じ通信回線を利用する際に、データの衝突を防ぎながら送信を行うための仕組みである。CSMA/CD方式は、通信路が空いていることを確認して送信し、衝突が発生した場合は一定時間待って再送する方式である。構造が簡単でコストが低く、通信量が少ない場合は効率がよい。一方、トークンパッシング方式は、トークンを持つ機器だけが送信できる方式であり、データの衝突が発生しないため、通信量が多い場合でも安定した通信が可能であるが、管理が複雑でコストが高い。`,

keywords:[
"CSMA/CD",
"トークン",
"衝突",
"再送",
"通信路",
"アクセス制御",
"コスト",
"安定"
]

},

{
number:4,

question:`イーサネットはなぜ盗聴のリスクが高いのか、説明せよ。`,

model:`イーサネットでは、同じネットワーク内に接続された機器へデータが送信される仕組みが用いられている。そのため、本来の宛先以外の機器にも通信データが届く場合があり、悪意のある利用者が通信内容を取得すると盗聴される可能性がある。このような性質から、イーサネットでは暗号化や認証などのセキュリティ対策を行うことが重要である。`,

keywords:[
"盗聴",
"暗号化",
"認証",
"通信",
"ネットワーク",
"宛先",
"セキュリティ"
]

},

{
number:5,

question:`プライベートIPアドレスの概要を、アドレス枯渇問題と関連させつつ、説明せよ。`,

model:`IPv4ではIPアドレスが32ビットで構成されており、利用できるアドレス数は約43億個に限られる。そのため、インターネット利用者や機器の増加により、IPアドレスが不足するアドレス枯渇問題が生じた。この問題への対策の一つがプライベートIPアドレスであり、家庭や企業などの内部ネットワークでのみ使用される。外部と通信する際は、NATによってプライベートIPアドレスをグローバルIPアドレスに変換することで、限られたグローバルIPアドレスを効率よく利用できる。`,

keywords:[
"IPv4",
"32",
"枯渇",
"NAT",
"プライベート",
"グローバル",
"43億"
]

},
{
number:6,

question:`TCPポート番号の主要な役割を1つあげて、それについて具体的に説明せよ。また、TCPとUDPの違いについて述べよ。`,

model:`TCPポート番号の役割は、通信データを適切なアプリケーションへ届けることである。例えば、Web通信には80番や443番、メールには25番など、サービスごとにポート番号が割り当てられているため、受信したデータを正しいアプリケーションで処理できる。TCPは通信相手との確認を行いながら送受信するため信頼性が高く、メールやWebページの閲覧などに利用される。一方、UDPは確認を行わずに通信するため高速であり、動画配信やIP電話などリアルタイム性が求められる通信に適している。`,

keywords:[
"ポート番号",
"アプリケーション",
"80",
"443",
"TCP",
"UDP",
"信頼性",
"高速"
]

},

{
number:7,

question:`システムポート（Well Knownポート）とは何か、説明せよ。`,

model:`システムポート（Well Knownポート）とは、よく利用されるネットワークサービスに対してあらかじめ割り当てられているポート番号であり、0番から1023番までの番号を指す。これらは国際的に標準化されているため、異なるコンピュータやOS同士でも同じサービスを利用できる。代表例として、HTTPは80番、HTTPSは443番、SMTPは25番などがある。`,

keywords:[
"Well Known",
"0",
"1023",
"HTTP",
"HTTPS",
"SMTP",
"80",
"443"
]

},

{
number:8,

question:`DNSの役割について、その具体的な動作を交えて説明せよ。`,

model:`DNS（Domain Name System）は、ドメイン名とIPアドレスを対応付ける仕組みである。利用者がWebサイトのドメイン名を入力すると、DNSサーバに問い合わせが行われ、対応するIPアドレスが検索される。そのIPアドレスをもとに通信先のコンピュータが特定され、目的のWebサイトへ接続できる。人間が覚えやすいドメイン名を利用できるのはDNSがあるためである。`,

keywords:[
"DNS",
"ドメイン",
"IPアドレス",
"名前解決",
"DNSサーバ",
"検索",
"Web"
]

},

{
number:9,

question:`メールのプロトコルがなぜSMTPとPOP3に分かれているのか、説明せよ。`,

model:`SMTPとPOP3は、それぞれ異なる役割を担うため分かれている。SMTPはメールを送信するためのプロトコルであり、送信者からメールサーバやメールサーバ間でメールを転送する際に利用される。一方、POP3はメールサーバに保存されたメールを受信者の端末へ取り込むためのプロトコルである。メールの送信と受信では必要な処理が異なるため、それぞれに適したプロトコルが用いられている。`,

keywords:[
"SMTP",
"POP3",
"送信",
"受信",
"メールサーバ",
"転送"
]

},

{
number:10,

question:`HTMLがどうしてメタ情報を含んでいるのか、自分の考察を交えて説明せよ。`,

model:`HTMLのメタ情報には、文字コードやページの概要、作成者、検索エンジン向けの情報など、画面には表示されない情報が記述される。これにより、ブラウザや検索エンジンがWebページの内容を正しく理解し、適切に表示・検索できるようになる。私は、Web上には膨大なページが存在するため、メタ情報を利用して内容を整理し、効率よく検索・管理できるようにすることが重要であると考える。`,

keywords:[
"メタ情報",
"文字コード",
"検索エンジン",
"ブラウザ",
"表示",
"検索",
"管理"
]

},
{
number:11,

question:`ハイパーテキストの普及がどのように社会を変えたか、自分の考察を交えて説明せよ。`,

model:`ハイパーテキストとは、文章中にリンクを埋め込み、関連する別のページへ移動できる仕組みである。この技術の普及によって、利用者は必要な情報へ素早くアクセスできるようになり、インターネットの利便性が大きく向上した。また、企業だけでなく個人でも容易に情報を発信・共有できるようになり、Webサイトやブログ、SNSなどの発展につながった。私は、ハイパーテキストは現在の情報社会を支える重要な技術の一つであると考える。`,

keywords:[
"ハイパーテキスト",
"リンク",
"情報",
"Web",
"SNS",
"発信",
"共有",
"利便性"
]

},

{
number:12,

question:`3ウェイハンドシェイクについて、説明せよ。`,

model:`3ウェイハンドシェイクとは、TCP通信を開始する前に送信側と受信側がお互いに通信可能であることを確認するための手順である。まず送信側が接続要求（SYN）を送り、受信側は接続を承認する応答（SYN+ACK）を返す。最後に送信側が確認応答（ACK）を返すことで接続が確立され、通信が開始される。この手順により、通信の信頼性を高めることができる。`,

keywords:[
"TCP",
"SYN",
"SYN+ACK",
"ACK",
"接続",
"通信",
"確認"
]

},

{
number:13,

question:`OSI基本参照モデルの第7層の役割について、説明せよ。`,

model:`OSI基本参照モデルの第7層はアプリケーション層であり、利用者が直接使用するアプリケーションとネットワークとの間で通信を行うための機能を提供する層である。電子メールやWeb閲覧、ファイル転送などのサービスで利用され、HTTPやSMTP、FTPなどのアプリケーションプロトコルが動作する。利用者が最も意識する機会の多い層である。`,

keywords:[
"アプリケーション層",
"HTTP",
"SMTP",
"FTP",
"Web",
"電子メール",
"第7層"
]

},

{
number:14,

question:`IPv6が開発された最も重要な理由と、それによってIPv4から改善された技術的な特徴を1つ挙げて説明せよ。`,

model:`IPv6が開発された最大の理由は、IPv4で発生したIPアドレスの枯渇問題を解決するためである。IPv4は32ビットで約43億個のアドレスしか利用できないが、IPv6では128ビットとなり、ほぼ無限に近い数のIPアドレスを割り当てられるようになった。そのため、インターネットに接続する機器が増加しても十分に対応できる。また、アドレスの自動設定機能なども備え、ネットワーク管理が容易になった。`,

keywords:[
"IPv6",
"128",
"IPv4",
"枯渇",
"アドレス",
"自動設定",
"43億"
]

},

{
number:15,

question:`Wi-FiになぜWPAが必要とされるのか、説明せよ。`,

model:`Wi-Fiは電波を利用して通信を行うため、通信範囲内であれば第三者にも電波を受信される可能性がある。そのため、暗号化を行わないと通信内容が盗聴されたり、不正アクセスを受けたりする危険がある。WPAはWi-Fi通信を暗号化し、利用者を認証するためのセキュリティ方式であり、安全に無線LANを利用するために必要である。現在では、より安全性を高めたWPA2やWPA3も広く利用されている。`,

keywords:[
"WPA",
"暗号化",
"認証",
"Wi-Fi",
"盗聴",
"無線LAN",
"WPA2",
"WPA3"
]

},

{
number:16,

question:`プライベートIPアドレスの概要を、NATと関連させて説明せよ。`,

model:`プライベートIPアドレスとは、家庭や企業などの内部ネットワークでのみ利用されるIPアドレスであり、そのままではインターネットへ直接接続できない。そこで利用されるのがNATであり、内部のプライベートIPアドレスを外部通信時にグローバルIPアドレスへ変換する仕組みである。これにより、限られた数のグローバルIPアドレスを複数の端末で共有でき、IPv4アドレスを効率的に利用できる。`,

keywords:[
"プライベートIP",
"NAT",
"グローバルIP",
"変換",
"内部",
"IPv4"
]

}

];
let order = [...questions];

let current = 0;

let scores = new Array(questions.length).fill(null);

function showQuestion(){

    document.getElementById("result").style.display="none";

    document.getElementById("modelAnswer").style.display="none";

    document.getElementById("answer").value="";

    document.getElementById("questionTitle").innerHTML=
    "問"+order[current].number;

    document.getElementById("questionText").innerHTML=
    order[current].question;

    document.getElementById("progress").innerHTML=
    "問題 "+(current+1)+" / "+order.length;

}

function calcScore(user,keywords){

    let point=0;

    let advice=[];

    keywords.forEach(k=>{

        if(user.includes(k)){

            point+=10;

        }else{

            advice.push("「"+k+"」について書くとより良くなります。");

        }

    });

    if(user.length>=250){

        point+=10;

    }else{

        advice.push("文字数が少なめです。具体例や理由を書くと高得点になります。");

    }

    if(point>100){

        point=100;

    }

    return{

        score:point,

        advice:advice

    };

}

document
.getElementById("checkBtn")
.onclick=function(){

    const text=
    document
    .getElementById("answer")
    .value;

    const result=
    calcScore(
        text,
        order[current].keywords
    );

    scores[current]=result.score;

    document
    .getElementById("result")
    .style.display="block";

    document
    .getElementById("score")
    .innerHTML=
    result.score+" / 100 点";

    let msg="";

    if(result.score>=90){

        msg+="<p class='good'>非常によく書けています。</p>";

    }

    else if(result.score>=70){

        msg+="<p class='good'>概ね良好です。</p>";

    }

    else{

        msg+="<p class='bad'>重要語句が不足しています。</p>";

    }

    result.advice.forEach(a=>{

        msg+="<div>"+a+"</div>";

    });

    document
    .getElementById("advice")
    .innerHTML=msg;

};

document
.getElementById("answerBtn")
.onclick=function(){

    const area=
    document
    .getElementById("modelAnswer");

    area.style.display="block";

    area.innerHTML=
    "<h2>解答例</h2><br>"+order[current].model;

};

document
.getElementById("nextBtn")
.onclick=function(){

    current++;

    if(current>=order.length){

        finish();

        return;

    }

    showQuestion();

};

document
.getElementById("shuffleBtn")
.onclick=function(){

    for(let i=order.length-1;i>0;i--){

        let j=Math.floor(
            Math.random()*(i+1)
        );

        [order[i],order[j]]=
        [order[j],order[i]];

    }

    current=0;

    scores=
    new Array(order.length).fill(null);

    showQuestion();

};
function finish(){

    document.querySelector(".card").style.display="block";

    document.getElementById("result").style.display="none";

    document.getElementById("modelAnswer").style.display="none";

    document.getElementById("questionTitle").style.display="none";

    document.getElementById("questionText").style.display="none";

    document.getElementById("answer").style.display="none";

    document.querySelector(".buttons").style.display="none";

    document.getElementById("progress").style.display="none";

    document.getElementById("finalResult").style.display="block";

    let total=0;

    let count=0;

    let table="";

    for(let i=0;i<order.length;i++){

        let s=scores[i];

        if(s===null){

            s=0;

        }

        total+=s;

        count++;

        table+=`
        <tr>
            <td>問${order[i].number}</td>
            <td>${s} 点</td>
        </tr>
        `;

    }

    const finalScore=
    Math.round(total/count);

    document.getElementById("totalScore").innerHTML=
    finalScore+" / 100 点";

    document.getElementById("scoreTable").innerHTML=
    table;

    let comment="";

    if(finalScore>=90){

        comment="素晴らしいです。本番でも十分に高得点が狙えます。";

    }else if(finalScore>=80){

        comment="よく理解できています。細かい語句を補強するとさらに良くなります。";

    }else if(finalScore>=70){

        comment="基礎はできています。重要キーワードを意識して記述量を増やしましょう。";

    }else if(finalScore>=60){

        comment="あと一歩です。解答例を見ながら重要語句を復習しましょう。";

    }else{

        comment="基本事項の復習をおすすめします。まずは各問題のキーワードを覚えましょう。";

    }

    document.getElementById("totalScore").innerHTML+=
    "<br><br><span style='font-size:22px;color:#333;'>"+comment+"</span>";

}

showQuestion();

</script>

</body>

</html>
