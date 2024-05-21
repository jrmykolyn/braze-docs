---
nav_title: トランザクションメールキャンペーン
article_title: トランザクションメールキャンペーン
page_order: 10

description: "このリファレンス記事では、Brazeトランザクションメールキャンペーンの新規作成と設定方法について説明します。"
page_type: reference
tool:
  - Campaigns
channel: email
alias: "/api/api_campaigns/transactional_campaigns"

---

# 取引メールキャンペーン

> Brazeのトランザクションメールは、送信者と受信者の間で合意されたトランザクションを促進するために送信されます。この参考記事では、Brazeのダッシュボードでトランザクションメールキャンペーンを作成し、[`/transactional/v1/campaigns/{campaign_id}/send` エンドポイントの]({{site.baseurl}}/api/endpoints/messaging/send_messages/post_send_transactional_message)APIコールに含める`campaign_id` を生成する方法について説明します。

{% alert important %}
Brazeトランザクションメールは、一部のBrazeパッケージでのみご利用いただけます。詳細については、Brazeカスタマーサクセスマネージャーにお問い合わせいただくか、[サポートチケットを]({{site.baseurl}}/braze_support/)ご請求ください。
{% endalert %}

トランザクショナルEメールキャンペーンは、お客様とお客様の間で合意された取引を促進するために、自動化された非プロモーションEメールメッセージを送信することを目的としています。これには以下のような情報が含まれる：

- 注文確認
- パスワードリセット
- 請求アラート
- 出荷アラート

つまり、トランザクションメールは、スピードが最重要視される単一ユーザー向けのサービスから発信されるビジネスクリティカルな通知を送信するために使用することができます。 

{% alert important %}
トランザクションメールはトランザクションキャンペーンとは異なり、追加コストをかけずにユーザーをターゲットにすることができます。例えば、トランザクショナルキャンペーンには、ユーザーが商品をカートに入れた後に送信されるメッセージを含めることができます。詳しくは、[オーディエンスターゲティングオプションを]({{site.baseurl}}/user_guide/engagement_tools/campaigns/building_campaigns/targeting_users/)ご覧ください。
{% endalert %}

## ステップ 1:新しいキャンペーンを作成する

新しいトランザクションメールキャンペーンを作成するには、キャンペーンを作成し、メッセージングチャネルとして**トランザクションメールを**選択します。

![トランザクションメール用にハイライトされたオプションでキャンペーンドロップダウンを作成][1]。{: style="float:right;max-width:30%;margin-left:15px;"}

トランザクションメールキャンペーンの設定に移ります。

## ステップ 2:キャンペーンを設定する

トランザクションメールキャンペーンのキャンペーン作成フローは、[通常のメールキャンペーンよりも]({{site.baseurl}}/user_guide/message_building_by_channel/email/html_editor/creating_an_email_campaign/)簡素化されており、ビジネスクリティカルなトランザクションメールがすべてのユーザーに届くようになっています。

その結果、他のBrazeキャンペーンタイプでお馴染みのいくつかの設定が、このキャンペーンタイプを設定する際には必要ないことにお気づきでしょう：

- **配信**ステップは簡素化され、スケジューリングオプションが削除された。トランザクションメールは、**配信**ページに表示されるキャンペーンIDを使用して、常にBraze REST APIを介してトリガーされます。サービスが送信リクエストをトリガーしたときに、すべてのユーザーがこれらの重要なトランザクションアラートに到達可能であることを確認するために、再資格コントロールや回数上限設定などの追加設定も削除されました。
- **ターゲットユーザー**」ステップは削除されました。トランザクショナルメールは、（配信停止ユーザーを含む）全ユーザーを対象として登録するため、フィルターやセグメントを指定する必要はありません。そのため、このメッセージを受け取るべきロジックがある場合は、そのロジックを適用してから、BrazeにAPIリクエストを行い、特定のユーザーにメッセージをトリガーするかどうかを決定することをお勧めします。
- **コンバージョン・ステップは**削除された。現時点では、トランザクションメールはコンバージョンイベントのトラッキングをサポートしていません。

トランザクションメールキャンペーンを作成するための作成、配信、確認ワークフロー][2]。

トランザクションメールキャンペーンを設定するには、以下の手順に従ってください：

1. メッセージを送信した後、**キャンペーン**ページで結果を見つけられるように、説明的な名前を追加します。
2. メールを作成するか、テンプレートから選択します。
3. `campaign_id` をメモしておこう。APIキャンペーンを保存した後、[Transactional Emailのエンドポイントの]({{site.baseurl}}/api/endpoints/messaging/send_messages/post_send_transactional_message)記事に記載されているように、生成された`campaign_id` フィールドをAPIリクエストに含める必要があります。
4. **Save Campaignを**クリックすると、APIキャンペーンが開始されます！

### トランザクションメールで許可されていないタグ

`Connected Content` および`Promotion Code` Liquid タグは、トランザクションメールキャンペーンではご利用いただけません。

`Connected Content` タグを使用すると、送信処理中にBrazeが送信APIリクエストを行う必要があり、リクエストした外部サービスに遅延が発生している場合、メッセージ送信処理が遅くなる可能性があります。同様に、`Promotion Code` タグを使用すると、Braze は送信前にプロモーションの有無を評価するための追加処理を行う必要があるため、プロモーションがない場合は送信処理が遅くなる可能性があります。

そのため、`Connected Content` または`Promotion Code` タグをトランザクションメールキャンペーンのフィールドに含めることはできません。


[1]: {% image_buster /assets/img/transactional_email_campaign.png %}
[2]: {% image_buster /assets/img/transactional_campaign_compose.png %}