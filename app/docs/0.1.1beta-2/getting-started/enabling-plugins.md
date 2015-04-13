---
title: Enabling Plugins
---

# Enabling Plugins

<div class="alert alert-warning">
  <strong>Before you start:</strong>
  <ol>
    <li>Make sure you've <a href="/install">installed Kong</a> - It should only take a minute!</li>
    <li>Make sure you've <a href="/docs/getting-started/quickstart">started Kong</a>.</li>
    <li>Also, make sure you've <a href="/docs/getting-started/added-your-api">added your API to Kong</a>.</li>
  </ol>
</div>

In this section, you'll learn how to enable plugins. One of the core principals of Kong is its extensibility through [plugins][plugins]. Plugins allow you to easily add new features to your API or make your API easier to manage.

First, we'll have you configure and enable the [keyauth][keyauth] plugin to add authentication to your API.

1. ### Add plugin to your Kong config

    Add `keyauth` under the `plugins_available` property in your Kong instance configuration file:

    ```yaml
    plugins_available:
     - keyauth
    ```

2. ### Restart Kong

    Issue the following command to restart Kong. This allows Kong to load the plugin.

    ```bash
    $ kong restart
    ```

    **Note:** If you have a cluster of Kong instances that share the configuration, you should restart each node in the cluster.

3. ### Configure the plugin for your API

    Now that Kong has loaded the plugin, we should configure it to be enabled on your API.

    Issue the following cURL request with your API `id` you created previously:

    ```bash
    $ curl -i -X POST \
       --url http://localhost:8001/plugins_configurations/ \
       --data 'name=keyauth' \
       --data 'api_id=YOUR_API_ID' \
       --data 'value.key_names=apikey'
    ```

    **Note:** `value.key_names` is the authentication header name each request will require.

4. ### Verify that the plugin is enabled for your API

    Issue the following cURL request to verify that the [keyauth][keyauth] plugin was enabled for your API:

    ```bash
    $ curl -i -X GET \
     --url http://localhost:8000/ \
     --header 'Host: mockbin.com'
    ```

    Since you did not specify the required `apikey` header the response should be `403 Forbidden`:

    ```http
    HTTP/1.1 403 Forbidden
    ...

    {
     "message": "Your authentication credentials are invalid"
    }
    ```

### Next Steps

Now that you've enabled the [keyauth][keyauth] plugin lets learn how to add consumers to your API so we can continue proxying requests through Kong.

Go to [Adding Consumers][adding-consumers]

[mockbin]: https://mockbin.com
[CLI]: /docs/cli
[API]: /docs/api
[keyauth]: /plugins/keyauth
[install]: /download
[plugins]: /plugins
[configuration]: /download
[migrations]: /docs/migrations
[quickstart]: /docs/getting-started/quickstart
[enabling-plugins]: /docs/getting-started/enabling-plugins
[adding-consumers]: /docs/getting-started/adding-consumers