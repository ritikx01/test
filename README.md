**Seeking an opportunity in the field of software engineering. Good knowledge of Python, Linux, Django. Ability to implement a full software development life cycle (SDLC) and analyze the performance of programs to correct deficiencies.**


<div align="center"> 
  <a href="https://linkedin.com/in/ritikx01" target="_blank"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white" target="_blank" /></a>
  <a href="https://twitter.com/wh15k3yTF"><img src="https://img.shields.io/badge/X-333333?style=for-the-badge&logo=X&logoColor=white" /></a>
  <a href="https://ritik.dev" target="_blank"><img src="https://img.shields.io/badge/Portfolio-FF5722?style=for-the-badge&logo=todoist&logoColor=white" target="_blank" /></a>
  <a href="mailto:hello@ritik.dev"><img src="https://img.shields.io/badge/Email-333333?style=for-the-badge&logo=gmail&logoColor=red" />
  </a>
  <a href="https://ritik.dev" target="_blank"><img src="https://img.shields.io/badge/Leetcode-FFA116?style=for-the-badge&logo=leetcode&logoColor=white" target="_blank" /></a>
</div>

<div align="center"> 
  <a href="https://www.youtube.com/channel/UCUrghtGYhkas_MoGzgSKaQA?app=desktop" target="_blank"><img src="https://img.shields.io/badge/YouTube-FF0000?style=for-the-badge&logo=youtube&logoColor=white" target="_blank"></a>
 <a href="https://discord.gg/wagxzStdcR" target="_blank"><img src="https://img.shields.io/badge/Discord-7289DA?style=for-the-badge&logo=discord&logoColor=white" target="_blank"></a> 
  <a href = "mailto:kushaltanna01@gmail.com"><img src="https://img.shields.io/badge/-Gmail-%23333?style=for-the-badge&logo=gmail&logoColor=white" target="_blank"></a>
  <a href="https://in.linkedin.com/in/kushal-tanna-870ba7215" target="_blank"><img src="https://img.shields.io/badge/-LinkedIn-%230077B5?style=for-the-badge&logo=linkedin&logoColor=white" target="_blank"></a> 
 
</div>

  
```mermaid
graph LR
    subgraph External Services
        subgraph BinanceAPI[Binance API]
            direction LR
            B_REST[REST API<br>/fapi/v1/exchangeInfo<br>/fapi/v1/klines<br>/fapi/v1/aggTrades]
            B_WS[WebSocket API<br>wss://fstream.binance.com/ws]
        end
        DiscordWebhook[Discord Webhook]
    end

    subgraph Core Application
        AppInit("App Initialization") --> MDM("Market Data Manager");
        MDM -->|Requests Data For Pair/Interval| DataAcq("Data Acquisition Service");

        DataAcq -- Request History --> B_REST;
        B_REST -- Historical Klines --> DataAcq;
        DataAcq -- Request Symbols --> B_REST;
        B_REST -- Active Trading Pairs --> DataAcq;

        DataAcq -- Connect & Subscribe --> B_WS;
        B_WS -- Real-time Klines Stream --> DataAcq;

        DataAcq -->|Raw Market Data| MDM;
        MDM -- Holds --> Store(["Global In-Memory<br>Market Data<br>(Prices, Volumes, EMA, Median)"]);
        MDM -->|New Candle Data| AlgoMgr("Algorithm Manager");

        AlgoMgr -->|Evaluate Strategies<br>(Volume Spike, Above EMA)| AlgoMgr;
        AlgoMgr -->|Signal Candidate<br>(Pair/Interval)| RTV("Realtime Signal Validator (WIP)");
    end

    subgraph Validation and Persistence (WIP)
       direction TB
       PGDB[("Postgres DB (WIP)")]
       OldVal("Old Signal Validator (WIP)");
       RTV -->|Validate Signal Using DB?| PGDB;
       PGDB -->|Historical/Reference Data?| RTV;
       RTV -->|Update Validated Signals| PGDB;
       RTV -->|Info for Old Validation?| OldVal;
       PGDB -->|Data for Old Validation?| OldVal;
    end

    subgraph Outputs
       direction TB
       Broadcast("Broadcast Service (WS)");
       Notifier("Discord Notifier");
       RTV -->|Validated Signal| Broadcast;
       RTV -->|Validated Signal| Notifier;
       Notifier -->|Send Notification| DiscordWebhook;
    end

    %% Styling for WIP components
    style RTV stroke-dasharray: 5 5, fill:#f9f,stroke:#333,stroke-width:2px;
    style PGDB stroke-dasharray: 5 5, fill:#ccf,stroke:#333,stroke-width:2px;
    style OldVal stroke-dasharray: 5 5, fill:#f9f,stroke:#333,stroke-width:2px;
```
