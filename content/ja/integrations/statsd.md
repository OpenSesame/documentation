---
assets:
  dashboards: {}
  logs: {}
  monitors: {}
  service_checks: assets/service_checks.json
categories:
  - monitoring
  - autodiscovery
creates_events: false
ddtype: check
dependencies:
  - 'https://github.com/DataDog/integrations-core/blob/master/statsd/README.md'
display_name: StatsD
git_integration_title: statsd
guid: 4830acf3-626b-42ff-a1db-3f37babd0ae6
integration_id: statsd
integration_title: StatsD
is_public: true
kind: インテグレーション
maintainer: help@datadoghq.com
manifest_version: 1.0.0
metric_prefix: statsd.
metric_to_check: statsd.counters.count
name: statsd
public_title: Datadog-StatsD インテグレーション
short_description: StatsD サーバーの可用性を監視し、メトリクスカウントを追跡。
support: コア
supported_os:
  - linux
  - mac_os
  - windows
---
## 概要

このチェックは、Datadog 以外の StatsD サーバーの可用性とアップタイムを監視します。また、StatsD が受け取ったメトリクス数をメトリクスタイプ別に追跡します。

このチェックは、アプリケーションメトリクスを StatsD サーバーから Datadog に**転送しません**。これは、StatsD 自体に関するメトリクスを収集します。

## セットアップ

### インストール

StatsD チェックは [Datadog Agent][1] パッケージに含まれています。StatsD を実行するサーバーに追加でインストールする必要はありません。

### 構成

{{< tabs >}}
{{% tab "Host" %}}

#### ホスト

ホストで実行中の Agent に対してこのチェックを構成するには:

1. [Agent の構成ディレクトリ][1]のルートにある `conf.d/` フォルダーの `statsd.d/conf.yaml` を編集します。使用可能なすべての構成オプションの詳細については、[サンプル statsd.d/conf.yaml][2] を参照してください。

   ```yaml
   init_config:

   instances:
     - host: localhost
       port: 8126 # or wherever your statsd listens
   ```

2. [Agent を再起動][3]すると、Datadog への StatsD メトリクスおよびサービスチェックの送信が開始されます。

[1]: https://docs.datadoghq.com/ja/agent/guide/agent-configuration-files/#agent-configuration-directory
[2]: https://github.com/DataDog/integrations-core/blob/master/statsd/datadog_checks/statsd/data/conf.yaml.example
[3]: https://docs.datadoghq.com/ja/agent/guide/agent-commands/#start-stop-and-restart-the-agent
{{% /tab %}}
{{% tab "Containerized" %}}

#### コンテナ化

コンテナ環境の場合は、[オートディスカバリーのインテグレーションテンプレート][1]のガイドを参照して、次のパラメーターを適用してください。

| パラメーター            | 値                                 |
| -------------------- | ------------------------------------- |
| `<インテグレーション名>` | `statsd`                              |
| `<初期コンフィギュレーション>`      | 空白または `{}`                         |
| `<インスタンスコンフィギュレーション>`  | `{"host": "%%host%%", "port":"8126"}` |

[1]: https://docs.datadoghq.com/ja/agent/kubernetes/integrations/
{{% /tab %}}
{{< /tabs >}}

### 検証

[Agent の `status` サブコマンドを実行][2]し、Checks セクションで `statsd` を探します。

## 収集データ

### メトリクス
{{< get-metrics-from-git "statsd" >}}


### イベント

StatsD チェックには、イベントは含まれません。

### サービスのチェック

**statsd.is_up**:

StatsD サーバーが Agent の健全性ステータスリクエストに応答しない場合は、CRITICAL を返します。それ以外の場合は、OK を返します。

**statsd.can_connect**:

Agent が StatsD に関するメトリクスを収集できない場合は、CRITICAL を返します。それ以外の場合は、OK を返します。

## トラブルシューティング

ご不明な点は、[Datadog のサポートチーム][3]までお問合せください。

## その他の参考資料

StatsD およびその動作の詳細については、[StatsD に関するブログ記事][4]を参照してください。

Datadog を使用して StatsD メトリクスをカウントグラフで視覚化する方法 (またはその理由) について理解するには、Datadog の[一連のブログ記事][5]を参照してください。


[1]: https://app.datadoghq.com/account/settings#agent
[2]: https://docs.datadoghq.com/ja/agent/guide/agent-commands/#agent-status-and-information
[3]: https://docs.datadoghq.com/ja/help/
[4]: https://www.datadoghq.com/blog/statsd
[5]: https://www.datadoghq.com/blog/visualize-statsd-metrics-counts-graphing