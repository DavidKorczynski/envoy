1.19.0 (Pending)
================

Incompatible Behavior Changes
-----------------------------
*Changes that are expected to cause an incompatibility if applicable; deployment changes are likely required*

* tracing: update Apache SkyWalking tracer version to be compatible with 8.4.0 data collect protocol. This change will introduce incompatibility with SkyWalking 8.3.0.

Minor Behavior Changes
----------------------
*Changes that may cause incompatibilities for some users, but should not for most*

* http: replaced setting ``envoy.reloadable_features.strict_1xx_and_204_response_headers`` with settings
  ``envoy.reloadable_features.require_strict_1xx_and_204_response_headers``
  (require upstream 1xx or 204 responses to not have Transfer-Encoding or non-zero Content-Length headers) and
  ``envoy.reloadable_features.send_strict_1xx_and_204_response_headers``
  (do not send 1xx or 204 responses with these headers). Both are true by default.
* listener: respect the :ref:`connection balance config <envoy_v3_api_field_config.listener.v3.Listener.connection_balance_config>`
  defined within the listener where the sockets are redirected to. Clear that field to restore the previous behavior.


Bug Fixes
---------
*Changes expected to improve the state of the world and are unlikely to have negative effects*

* http: port stripping now works for CONNECT requests, though the port will be restored if the CONNECT request is sent upstream. This behavior can be temporarily reverted by setting ``envoy.reloadable_features.strip_port_from_connect`` to false.
* http: raise max configurable max_request_headers_kb limit to 8192 KiB (8MiB) from 96 KiB in http connection manager.
* validation: fix an issue that causes TAP sockets to panic during config validation mode.
* xray: fix the default sampling 'rate' for AWS X-Ray tracer extension to be 5% as opposed to 50%.
* zipkin: fix timestamp serializaiton in annotations. A prior bug fix exposed an issue with timestamps being serialized as strings.

Removed Config or Runtime
-------------------------
*Normally occurs at the end of the* :ref:`deprecation period <deprecated>`

* http: removed ``envoy.reloadable_features.allow_500_after_100`` runtime guard and the legacy code path.
* http: removed ``envoy.reloadable_features.always_apply_route_header_rules`` runtime guard and legacy code path.
* http: removed ``envoy.reloadable_features.hcm_stream_error_on_invalid_message`` for disabling closing HTTP/1.1 connections on error. Connection-closing can still be disabled by setting the HTTP/1 configuration :ref:`override_stream_error_on_invalid_http_message <envoy_v3_api_field_config.core.v3.Http1ProtocolOptions.override_stream_error_on_invalid_http_message>`.
* http: removed ``envoy.reloadable_features.http_set_copy_replace_all_headers`` runtime guard and legacy code paths.
* http: removed ``envoy.reloadable_features.overload_manager_disable_keepalive_drain_http2``; Envoy will now always send GOAWAY to HTTP2 downstreams when the :ref:`disable_keepalive <config_overload_manager_overload_actions>` overload action is active.
* http: removed ``envoy.reloadable_features.http_match_on_all_headers`` runtime guard and legacy code paths.
* http: removed ``envoy.reloadable_features.unify_grpc_handling`` runtime guard and legacy code paths.
* tls: removed ``envoy.reloadable_features.tls_use_io_handle_bio`` runtime guard and legacy code path.

New Features
------------

* metric service: added support for sending metric tags as labels. This can be enabled by setting the :ref:`emit_tags_as_labels <envoy_v3_api_field_config.metrics.v3.MetricsServiceConfig.emit_tags_as_labels>` field to true.

Deprecated
----------
