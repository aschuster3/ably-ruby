# Ably Realtime & REST Client Library 0.9.0-pre.1 Specification

### Ably::Realtime::Auth
_(see [spec/acceptance/realtime/auth_spec.rb](./spec/acceptance/realtime/auth_spec.rb))_
  * using JSON protocol
    * with basic auth
      * #authentication_security_requirements_met?
        * [returns true](./spec/acceptance/realtime/auth_spec.rb#L28)
      * #key
        * [contains the API key](./spec/acceptance/realtime/auth_spec.rb#L35)
      * #key_name
        * [contains the API key name](./spec/acceptance/realtime/auth_spec.rb#L42)
      * #key_secret
        * [contains the API key secret](./spec/acceptance/realtime/auth_spec.rb#L49)
      * #using_basic_auth?
        * [is true when using Basic Auth](./spec/acceptance/realtime/auth_spec.rb#L56)
      * #using_token_auth?
        * [is false when using Basic Auth](./spec/acceptance/realtime/auth_spec.rb#L63)
    * with token auth
      * #client_id
        * [contains the ClientOptions client ID](./spec/acceptance/realtime/auth_spec.rb#L75)
      * #current_token_details
        * [contains the current token after auth](./spec/acceptance/realtime/auth_spec.rb#L82)
      * #token_renewable?
        * [is true when an API key exists](./spec/acceptance/realtime/auth_spec.rb#L92)
      * #options (auth_options)
        * [contains the configured auth options](./spec/acceptance/realtime/auth_spec.rb#L104)
      * #token_params
        * [contains the configured auth options](./spec/acceptance/realtime/auth_spec.rb#L115)
      * #using_basic_auth?
        * [is false when using Token Auth](./spec/acceptance/realtime/auth_spec.rb#L124)
      * #using_token_auth?
        * [is true when using Token Auth](./spec/acceptance/realtime/auth_spec.rb#L133)
    * methods
      * #create_token_request
        * [returns a token request asynchronously](./spec/acceptance/realtime/auth_spec.rb#L147)
      * #create_token_request_async
        * [returns a token request synchronously](./spec/acceptance/realtime/auth_spec.rb#L157)
      * #request_token
        * [returns a token asynchronously](./spec/acceptance/realtime/auth_spec.rb#L167)
      * #request_token_async
        * [returns a token synchronously](./spec/acceptance/realtime/auth_spec.rb#L178)
      * #authorize
        * with token auth
          * [returns a token asynchronously](./spec/acceptance/realtime/auth_spec.rb#L192)
        * with auth_callback blocking
          * with a slow auth callback response
            * [asynchronously authenticates](./spec/acceptance/realtime/auth_spec.rb#L215)
        * when implicitly called, with an explicit ClientOptions client_id
          * and an incompatible client_id in a TokenDetails object passed to the auth callback
            * [rejects a TokenDetails object with an incompatible client_id and fails with an exception](./spec/acceptance/realtime/auth_spec.rb#L239)
          * and an incompatible client_id in a TokenRequest object passed to the auth callback and fails with an exception
            * [rejects a TokenRequests object with an incompatible client_id and fails with an exception](./spec/acceptance/realtime/auth_spec.rb#L255)
        * when explicitly called, with an explicit ClientOptions client_id
          * and an incompatible client_id in a TokenDetails object passed to the auth callback
            * [rejects a TokenDetails object with an incompatible client_id and fails with an exception](./spec/acceptance/realtime/auth_spec.rb#L287)
        * when already authenticated with a valid token
          * [ensures message delivery continuity whilst upgrading (#RTC8a1)](./spec/acceptance/realtime/auth_spec.rb#L700)
          * when INITIALIZED
            * [obtains a token and connects to Ably (#RTC8c, #RTC8b1)](./spec/acceptance/realtime/auth_spec.rb#L328)
          * when CONNECTING
            * [aborts the current connection process, obtains a token, and connects to Ably again (#RTC8b)](./spec/acceptance/realtime/auth_spec.rb#L350)
          * when FAILED
            * [obtains a token and connects to Ably (#RTC8c, #RTC8b1)](./spec/acceptance/realtime/auth_spec.rb#L369)
          * when CLOSED
            * [obtains a token and connects to Ably (#RTC8c, #RTC8b1, #RTC8a3)](./spec/acceptance/realtime/auth_spec.rb#L386)
          * when in the CONNECTED state
            * with a valid token in the AUTH ProtocolMessage sent
              * PENDING: *[obtains a new token (that upgrades from anonymous to identified) and upgrades the connection after receiving an updated CONNECTED ProtocolMessage (#RTC8a, #RTC8a3)](./spec/acceptance/realtime/auth_spec.rb#L409)*
              * [obtains a new token (as anonymous user before & after) and upgrades the connection after receiving an updated CONNECTED ProtocolMessage (#RTC8a, #RTC8a3)](./spec/acceptance/realtime/auth_spec.rb#L445)
          * when DISCONNECTED
            * PENDING: *[obtains a token, upgrades from anonymous to identified, and connects to Ably immediately (#RTC8c, #RTC8b1)](./spec/acceptance/realtime/auth_spec.rb#L481)*
            * [obtains a similar anonymous token and connects to Ably immediately (#RTC8c, #RTC8b1)](./spec/acceptance/realtime/auth_spec.rb#L517)
          * when SUSPENDED
            * [obtains a token and connects to Ably immediately (#RTC8c, #RTC8b1)](./spec/acceptance/realtime/auth_spec.rb#L561)
          * when client is identified
            * [transitions the connection state to FAILED if the client_id changes (#RSA15c, #RTC8a2)](./spec/acceptance/realtime/auth_spec.rb#L596)
          * when auth fails
            * [transitions the connection state to the FAILED state (#RSA15c, #RTC8a2, #RTC8a3)](./spec/acceptance/realtime/auth_spec.rb#L611)
          * when the authCallback fails
            * [calls the error callback of authorize and leaves the connection intact (#RSA4c3)](./spec/acceptance/realtime/auth_spec.rb#L639)
          * when upgrading capabilities
            * [is allowed (#RTC8a1)](./spec/acceptance/realtime/auth_spec.rb#L658)
          * when downgrading capabilities (#RTC8a1)
            * [is allowed and channels are detached](./spec/acceptance/realtime/auth_spec.rb#L683)
      * #authorize_async
        * [returns a token synchronously](./spec/acceptance/realtime/auth_spec.rb#L734)
    * server initiated AUTH ProtocolMessage
      * when received
        * [should immediately start a new authentication process (#RTN22)](./spec/acceptance/realtime/auth_spec.rb#L758)
      * when not received
        * [should expect the connection to be disconnected by the server but should resume automatically (#RTN22a)](./spec/acceptance/realtime/auth_spec.rb#L781)
    * #auth_params
      * [returns the auth params asynchronously](./spec/acceptance/realtime/auth_spec.rb#L807)
    * #auth_params_sync
      * [returns the auth params synchronously](./spec/acceptance/realtime/auth_spec.rb#L816)
    * #auth_header
      * [returns an auth header asynchronously](./spec/acceptance/realtime/auth_spec.rb#L823)
    * #auth_header_sync
      * [returns an auth header synchronously](./spec/acceptance/realtime/auth_spec.rb#L832)
    * #client_id_validated?
      * when using basic auth
        * before connected
          * [is false as basic auth users do not have an identity](./spec/acceptance/realtime/auth_spec.rb#L845)
        * once connected
          * [is true](./spec/acceptance/realtime/auth_spec.rb#L852)
          * [contains a validated wildcard client_id](./spec/acceptance/realtime/auth_spec.rb#L859)
      * when using a token string
        * with a valid client_id
          * before connected
            * [is false as identification is not possible from an opaque token string](./spec/acceptance/realtime/auth_spec.rb#L873)
            * [#client_id is nil](./spec/acceptance/realtime/auth_spec.rb#L878)
          * once connected
            * [is true](./spec/acceptance/realtime/auth_spec.rb#L885)
            * [#client_id is populated](./spec/acceptance/realtime/auth_spec.rb#L892)
        * with no client_id (anonymous)
          * before connected
            * [is false as identification is not possible from an opaque token string](./spec/acceptance/realtime/auth_spec.rb#L905)
          * once connected
            * [is true](./spec/acceptance/realtime/auth_spec.rb#L912)
        * with a wildcard client_id (anonymous)
          * before connected
            * [is false as identification is not possible from an opaque token string](./spec/acceptance/realtime/auth_spec.rb#L925)
          * once connected
            * [is true](./spec/acceptance/realtime/auth_spec.rb#L932)
      * when using a token
        * with a client_id
          * [is true](./spec/acceptance/realtime/auth_spec.rb#L946)
          * once connected
            * [is true](./spec/acceptance/realtime/auth_spec.rb#L952)
        * with no client_id (anonymous)
          * [is true](./spec/acceptance/realtime/auth_spec.rb#L964)
          * once connected
            * [is true](./spec/acceptance/realtime/auth_spec.rb#L970)
        * with a wildcard client_id (anonymous)
          * [is true](./spec/acceptance/realtime/auth_spec.rb#L982)
          * once connected
            * [is true](./spec/acceptance/realtime/auth_spec.rb#L988)
      * when using a token request with a client_id
        * [is not true as identification is not confirmed until authenticated](./spec/acceptance/realtime/auth_spec.rb#L1001)
        * once connected
          * [is true as identification is completed following CONNECTED ProtocolMessage](./spec/acceptance/realtime/auth_spec.rb#L1007)
    * deprecated #authorise
      * [logs a deprecation warning (#RSA10l)](./spec/acceptance/realtime/auth_spec.rb#L1045)
      * [returns a valid token (#RSA10l)](./spec/acceptance/realtime/auth_spec.rb#L1051)

### Ably::Realtime::Channel#history
_(see [spec/acceptance/realtime/channel_history_spec.rb](./spec/acceptance/realtime/channel_history_spec.rb))_
  * using JSON protocol
    * [returns a SafeDeferrable that catches exceptions in callbacks and logs them](./spec/acceptance/realtime/channel_history_spec.rb#L21)
    * with a single client publishing and receiving
      * [retrieves realtime history](./spec/acceptance/realtime/channel_history_spec.rb#L34)
    * with two clients publishing messages on the same channel
      * [retrieves realtime history on both channels](./spec/acceptance/realtime/channel_history_spec.rb#L46)
    * with lots of messages published with a single client and channel
      * as one ProtocolMessage
        * [retrieves history forwards with pagination through :limit option](./spec/acceptance/realtime/channel_history_spec.rb#L88)
        * [retrieves history backwards with pagination through :limit option](./spec/acceptance/realtime/channel_history_spec.rb#L97)
      * in multiple ProtocolMessages
        * [retrieves limited history forwards with pagination](./spec/acceptance/realtime/channel_history_spec.rb#L108)
        * [retrieves limited history backwards with pagination](./spec/acceptance/realtime/channel_history_spec.rb#L119)
      * and REST history
        * [return the same results with unique matching message IDs](./spec/acceptance/realtime/channel_history_spec.rb#L135)
    * with option until_attach: true
      * [retrieves all messages before channel was attached](./spec/acceptance/realtime/channel_history_spec.rb#L160)
      * [fails the deferrable unless the state is attached](./spec/acceptance/realtime/channel_history_spec.rb#L209)
      * and two pages of messages
        * [retrieves two pages of messages before channel was attached](./spec/acceptance/realtime/channel_history_spec.rb#L175)

### Ably::Realtime::Channel
_(see [spec/acceptance/realtime/channel_spec.rb](./spec/acceptance/realtime/channel_spec.rb))_
  * using JSON protocol
    * initialization
      * with :auto_connect option set to false on connection
        * [remains initialized when accessing a channel](./spec/acceptance/realtime/channel_spec.rb#L26)
        * [opens a connection implicitly on #attach](./spec/acceptance/realtime/channel_spec.rb#L34)
    * #attach
      * [returns a SafeDeferrable that catches exceptions in callbacks and logs them](./spec/acceptance/realtime/channel_spec.rb#L139)
      * [calls the SafeDeferrable callback on success (#RTL4d)](./spec/acceptance/realtime/channel_spec.rb#L144)
      * when initialized
        * [emits attaching then attached events](./spec/acceptance/realtime/channel_spec.rb#L45)
        * [ignores subsequent #attach calls but calls the success callback if provided](./spec/acceptance/realtime/channel_spec.rb#L55)
        * [attaches to a channel](./spec/acceptance/realtime/channel_spec.rb#L68)
        * [attaches to a channel and calls the provided block (#RTL4d)](./spec/acceptance/realtime/channel_spec.rb#L76)
        * [sends an ATTACH and waits for an ATTACHED (#RTL4c)](./spec/acceptance/realtime/channel_spec.rb#L83)
        * [implicitly attaches the channel (#RTL7c)](./spec/acceptance/realtime/channel_spec.rb#L107)
        * when the implicit channel attach fails
          * [registers the listener anyway (#RTL7c)](./spec/acceptance/realtime/channel_spec.rb#L124)
      * when an ATTACHED acknowledge is not received on the current connection
        * [sends another ATTACH each time the connection becomes connected](./spec/acceptance/realtime/channel_spec.rb#L155)
      * when state is :attached
        * [does nothing (#RTL4a)](./spec/acceptance/realtime/channel_spec.rb#L193)
      * when state is :failed
        * [reattaches and sets the errorReason to nil (#RTL4g)](./spec/acceptance/realtime/channel_spec.rb#L213)
      * when state is :detaching
        * [does the attach operation after the completion of the pending request (#RTL4h)](./spec/acceptance/realtime/channel_spec.rb#L228)
      * with many connections and many channels on each simultaneously
        * [attaches all channels](./spec/acceptance/realtime/channel_spec.rb#L256)
      * failure as a result of insufficient key permissions
        * [emits failed event (#RTL4e)](./spec/acceptance/realtime/channel_spec.rb#L288)
        * [calls the errback of the returned Deferrable (#RTL4d)](./spec/acceptance/realtime/channel_spec.rb#L297)
        * [updates the error_reason](./spec/acceptance/realtime/channel_spec.rb#L305)
        * and subsequent authorisation with suitable permissions
          * [attaches to the channel successfully and resets the channel error_reason](./spec/acceptance/realtime/channel_spec.rb#L314)
      * with connection state
        * [is initialized (#RTL4i)](./spec/acceptance/realtime/channel_spec.rb#L333)
        * [is connecting (#RTL4i)](./spec/acceptance/realtime/channel_spec.rb#L340)
        * [is disconnected (#RTL4i)](./spec/acceptance/realtime/channel_spec.rb#L348)
    * #detach
      * when state is :attached
        * [it detaches from a channel (#RTL5d)](./spec/acceptance/realtime/channel_spec.rb#L363)
        * [detaches from a channel and calls the provided block (#RTL5d, #RTL5e)](./spec/acceptance/realtime/channel_spec.rb#L373)
        * [emits :detaching then :detached events](./spec/acceptance/realtime/channel_spec.rb#L383)
        * [returns a SafeDeferrable that catches exceptions in callbacks and logs them](./spec/acceptance/realtime/channel_spec.rb#L395)
        * [calls the Deferrable callback on success](./spec/acceptance/realtime/channel_spec.rb#L402)
        * and DETACHED message is not received within realtime request timeout
          * [fails the deferrable and returns to the previous state (#RTL5f, #RTL5e)](./spec/acceptance/realtime/channel_spec.rb#L416)
      * when state is :failed
        * [fails the deferrable (#RTL5b)](./spec/acceptance/realtime/channel_spec.rb#L436)
      * when state is :attaching
        * [waits for the attach to complete and then moves to detached](./spec/acceptance/realtime/channel_spec.rb#L449)
      * when state is :detaching
        * [ignores subsequent #detach calls but calls the callback if provided (#RTL5i)](./spec/acceptance/realtime/channel_spec.rb#L466)
      * when state is :suspended
        * [moves the channel state immediately to DETACHED state (#RTL5j)](./spec/acceptance/realtime/channel_spec.rb#L483)
      * when state is :initialized
        * [does nothing as there is no channel to detach (#RTL5a)](./spec/acceptance/realtime/channel_spec.rb#L503)
        * [returns a valid deferrable](./spec/acceptance/realtime/channel_spec.rb#L511)
      * when state is :detached
        * [does nothing as the channel is detached (#RTL5a)](./spec/acceptance/realtime/channel_spec.rb#L521)
      * when connection state is
        * closing
          * [fails the deferrable (#RTL5b)](./spec/acceptance/realtime/channel_spec.rb#L538)
        * failed and channel is failed
          * [fails the deferrable (#RTL5b)](./spec/acceptance/realtime/channel_spec.rb#L558)
        * failed and channel is detached
          * [fails the deferrable (#RTL5b)](./spec/acceptance/realtime/channel_spec.rb#L580)
        * initialized
          * [does the detach operation once the connection state is connected (#RTL5h)](./spec/acceptance/realtime/channel_spec.rb#L600)
        * connecting
          * [does the detach operation once the connection state is connected (#RTL5h)](./spec/acceptance/realtime/channel_spec.rb#L615)
        * disconnected
          * [does the detach operation once the connection state is connected (#RTL5h)](./spec/acceptance/realtime/channel_spec.rb#L634)
    * automatic channel recovery
      * when an ATTACH request times out
        * [moves to the SUSPENDED state (#RTL4f)](./spec/acceptance/realtime/channel_spec.rb#L661)
      * if a subsequent ATTACHED is received on an ATTACHED channel
        * [ignores the additional ATTACHED if resumed is true (#RTL12)](./spec/acceptance/realtime/channel_spec.rb#L675)
        * [emits an UPDATE only when resumed is true (#RTL12)](./spec/acceptance/realtime/channel_spec.rb#L689)
        * [emits an UPDATE when resumed is true and includes the reason error from the ProtocolMessage (#RTL12)](./spec/acceptance/realtime/channel_spec.rb#L705)
    * #publish
      * when attached
        * [publishes messages](./spec/acceptance/realtime/channel_spec.rb#L730)
      * when not yet attached
        * [publishes queued messages once attached](./spec/acceptance/realtime/channel_spec.rb#L742)
        * [publishes queued messages within a single protocol message](./spec/acceptance/realtime/channel_spec.rb#L750)
        * with :queue_messages client option set to false
          * and connection state initialized
            * [fails the deferrable](./spec/acceptance/realtime/channel_spec.rb#L773)
          * and connection state connecting
            * [fails the deferrable](./spec/acceptance/realtime/channel_spec.rb#L783)
          * and connection state disconnected
            * [fails the deferrable](./spec/acceptance/realtime/channel_spec.rb#L797)
          * and connection state connected
            * [publishes the message](./spec/acceptance/realtime/channel_spec.rb#L812)
      * with name and data arguments
        * [publishes the message and return true indicating success](./spec/acceptance/realtime/channel_spec.rb#L823)
        * and additional attributes
          * [publishes the message with the attributes and return true indicating success](./spec/acceptance/realtime/channel_spec.rb#L836)
        * and additional invalid attributes
          * [throws an exception](./spec/acceptance/realtime/channel_spec.rb#L849)
      * with an array of Hash objects with :name and :data attributes
        * [publishes an array of messages in one ProtocolMessage](./spec/acceptance/realtime/channel_spec.rb#L863)
      * with an array of Message objects
        * [publishes an array of messages in one ProtocolMessage](./spec/acceptance/realtime/channel_spec.rb#L891)
        * nil attributes
          * when name is nil
            * [publishes the message without a name attribute in the payload](./spec/acceptance/realtime/channel_spec.rb#L915)
          * when data is nil
            * [publishes the message without a data attribute in the payload](./spec/acceptance/realtime/channel_spec.rb#L938)
          * with neither name or data attributes
            * [publishes the message without any attributes in the payload](./spec/acceptance/realtime/channel_spec.rb#L961)
        * with two invalid message out of 12
          * before client_id is known (validated)
            * [calls the errback once](./spec/acceptance/realtime/channel_spec.rb#L985)
          * when client_id is known (validated)
            * [raises an exception](./spec/acceptance/realtime/channel_spec.rb#L1005)
        * only invalid messages
          * before client_id is known (validated)
            * [calls the errback once](./spec/acceptance/realtime/channel_spec.rb#L1024)
          * when client_id is known (validated)
            * [raises an exception](./spec/acceptance/realtime/channel_spec.rb#L1043)
      * with many many messages and many connections simultaneously
        * [publishes all messages, all success callbacks are called, and a history request confirms all messages were published](./spec/acceptance/realtime/channel_spec.rb#L1057)
      * identified clients
        * when authenticated with a wildcard client_id
          * with a valid client_id in the message
            * [succeeds](./spec/acceptance/realtime/channel_spec.rb#L1085)
          * with a wildcard client_id in the message
            * [throws an exception](./spec/acceptance/realtime/channel_spec.rb#L1097)
          * with a non-String client_id in the message
            * [throws an exception](./spec/acceptance/realtime/channel_spec.rb#L1104)
          * with an empty client_id in the message
            * [succeeds and publishes without a client_id](./spec/acceptance/realtime/channel_spec.rb#L1111)
        * when authenticated with a Token string with an implicit client_id
          * before the client is CONNECTED and the client's identity has been obtained
            * with a valid client_id in the message
              * [succeeds](./spec/acceptance/realtime/channel_spec.rb#L1131)
            * with an invalid client_id in the message
              * [succeeds in the client library but then fails when delivered to Ably](./spec/acceptance/realtime/channel_spec.rb#L1144)
            * with an empty client_id in the message
              * [succeeds and publishes with an implicit client_id](./spec/acceptance/realtime/channel_spec.rb#L1155)
          * after the client is CONNECTED and the client's identity is known
            * with a valid client_id in the message
              * [succeeds](./spec/acceptance/realtime/channel_spec.rb#L1169)
            * with an invalid client_id in the message
              * [throws an exception](./spec/acceptance/realtime/channel_spec.rb#L1183)
            * with an empty client_id in the message
              * [succeeds and publishes with an implicit client_id](./spec/acceptance/realtime/channel_spec.rb#L1192)
        * when authenticated with a valid client_id
          * with a valid client_id
            * [succeeds](./spec/acceptance/realtime/channel_spec.rb#L1214)
          * with a wildcard client_id in the message
            * [throws an exception](./spec/acceptance/realtime/channel_spec.rb#L1226)
          * with an invalid client_id in the message
            * [throws an exception](./spec/acceptance/realtime/channel_spec.rb#L1233)
          * with an empty client_id in the message
            * [succeeds and publishes with an implicit client_id](./spec/acceptance/realtime/channel_spec.rb#L1240)
        * when anonymous and no client_id
          * with a client_id in the message
            * [throws an exception](./spec/acceptance/realtime/channel_spec.rb#L1259)
          * with a wildcard client_id in the message
            * [throws an exception](./spec/acceptance/realtime/channel_spec.rb#L1266)
          * with an empty client_id in the message
            * [succeeds and publishes with an implicit client_id](./spec/acceptance/realtime/channel_spec.rb#L1273)
    * #subscribe
      * with an event argument
        * [subscribes for a single event](./spec/acceptance/realtime/channel_spec.rb#L1289)
      * before attach
        * [receives messages as soon as attached](./spec/acceptance/realtime/channel_spec.rb#L1299)
      * with no event argument
        * [subscribes for all events](./spec/acceptance/realtime/channel_spec.rb#L1313)
      * with a callback that raises an exception
        * [logs the error and continues](./spec/acceptance/realtime/channel_spec.rb#L1325)
      * many times with different event names
        * [filters events accordingly to each callback](./spec/acceptance/realtime/channel_spec.rb#L1344)
    * #unsubscribe
      * with an event argument
        * [unsubscribes for a single event](./spec/acceptance/realtime/channel_spec.rb#L1367)
      * with no event argument
        * [unsubscribes for a single event](./spec/acceptance/realtime/channel_spec.rb#L1380)
    * when connection state changes to
      * :failed
        * an :attaching channel
          * [transitions state to :failed (#RTL3a)](./spec/acceptance/realtime/channel_spec.rb#L1403)
        * an :attached channel
          * [transitions state to :failed (#RTL3a)](./spec/acceptance/realtime/channel_spec.rb#L1420)
          * [updates the channel error_reason (#RTL3a)](./spec/acceptance/realtime/channel_spec.rb#L1432)
        * a :detached channel
          * [remains in the :detached state (#RTL3a)](./spec/acceptance/realtime/channel_spec.rb#L1446)
        * a :failed channel
          * [remains in the :failed state and ignores the failure error (#RTL3a)](./spec/acceptance/realtime/channel_spec.rb#L1465)
        * a channel ATTACH request
          * [fails the deferrable (#RTL4b)](./spec/acceptance/realtime/channel_spec.rb#L1485)
      * :closed
        * an :attached channel
          * [transitions state to :detached (#RTL3b)](./spec/acceptance/realtime/channel_spec.rb#L1501)
        * an :attaching channel (#RTL3b)
          * [transitions state to :detached](./spec/acceptance/realtime/channel_spec.rb#L1512)
        * a :detached channel
          * [remains in the :detached state (#RTL3b)](./spec/acceptance/realtime/channel_spec.rb#L1527)
        * a :failed channel
          * [remains in the :failed state and retains the error_reason (#RTL3b)](./spec/acceptance/realtime/channel_spec.rb#L1547)
        * a channel ATTACH request when connection CLOSED
          * [fails the deferrable (#RTL4b)](./spec/acceptance/realtime/channel_spec.rb#L1567)
        * a channel ATTACH request when connection CLOSING
          * [fails the deferrable (#RTL4b)](./spec/acceptance/realtime/channel_spec.rb#L1581)
      * :suspended
        * an :attaching channel
          * [transitions state to :suspended (#RTL3c)](./spec/acceptance/realtime/channel_spec.rb#L1597)
        * an :attached channel
          * [transitions state to :suspended (#RTL3c)](./spec/acceptance/realtime/channel_spec.rb#L1611)
          * [transitions state automatically to :attaching once the connection is re-established (#RTN15c3)](./spec/acceptance/realtime/channel_spec.rb#L1620)
        * a :detached channel
          * [remains in the :detached state (#RTL3c)](./spec/acceptance/realtime/channel_spec.rb#L1634)
        * a :failed channel
          * [remains in the :failed state and retains the error_reason (#RTL3c)](./spec/acceptance/realtime/channel_spec.rb#L1654)
        * a channel ATTACH request when connection SUSPENDED (#RTL4b)
          * [fails the deferrable](./spec/acceptance/realtime/channel_spec.rb#L1676)
      * :connected
        * a :suspended channel
          * [is automatically reattached (#RTL3d)](./spec/acceptance/realtime/channel_spec.rb#L1692)
          * when re-attach attempt fails
            * [returns to a suspended state (#RTL3d)](./spec/acceptance/realtime/channel_spec.rb#L1709)
      * :disconnected
        * with an initialized channel
          * [has no effect on the channel states (#RTL3e)](./spec/acceptance/realtime/channel_spec.rb#L1735)
        * with an attaching channel
          * [has no effect on the channel states (#RTL3e)](./spec/acceptance/realtime/channel_spec.rb#L1748)
        * with an attached channel
          * [has no effect on the channel states (#RTL3e)](./spec/acceptance/realtime/channel_spec.rb#L1763)
        * with a detached channel
          * [has no effect on the channel states (#RTL3e)](./spec/acceptance/realtime/channel_spec.rb#L1775)
        * with a failed channel
          * [has no effect on the channel states (#RTL3e)](./spec/acceptance/realtime/channel_spec.rb#L1797)
    * #presence
      * [returns a Ably::Realtime::Presence object](./spec/acceptance/realtime/channel_spec.rb#L1812)
    * channel state change
      * [emits a ChannelStateChange object](./spec/acceptance/realtime/channel_spec.rb#L1819)
      * ChannelStateChange object
        * [has current state](./spec/acceptance/realtime/channel_spec.rb#L1828)
        * [has a previous state](./spec/acceptance/realtime/channel_spec.rb#L1837)
        * [has the event that generated the state change (#TA5)](./spec/acceptance/realtime/channel_spec.rb#L1846)
        * [has an empty reason when there is no error](./spec/acceptance/realtime/channel_spec.rb#L1864)
        * on failure
          * [has a reason Error object when there is an error on the channel](./spec/acceptance/realtime/channel_spec.rb#L1877)
        * #resume (#RTL2f)
          * [is false when a channel first attaches](./spec/acceptance/realtime/channel_spec.rb#L1890)
          * PENDING: *[is true when a connection is recovered and the channel is attached](./spec/acceptance/realtime/channel_spec.rb#L1898)*
          * [is false when a connection fails to recover and the channel is attached](./spec/acceptance/realtime/channel_spec.rb#L1919)
          * when a resume fails
            * [is false when a resume fails to recover and the channel is automatically re-attached](./spec/acceptance/realtime/channel_spec.rb#L1941)
      * moves to
        * suspended
          * [all queued messages fail with NACK (#RTL11)](./spec/acceptance/realtime/channel_spec.rb#L1960)
          * [all published messages awaiting an ACK do nothing (#RTL11a)](./spec/acceptance/realtime/channel_spec.rb#L1983)
        * detached
          * [all queued messages fail with NACK (#RTL11)](./spec/acceptance/realtime/channel_spec.rb#L1960)
          * [all published messages awaiting an ACK do nothing (#RTL11a)](./spec/acceptance/realtime/channel_spec.rb#L1983)
        * failed
          * [all queued messages fail with NACK (#RTL11)](./spec/acceptance/realtime/channel_spec.rb#L1960)
          * [all published messages awaiting an ACK do nothing (#RTL11a)](./spec/acceptance/realtime/channel_spec.rb#L1983)
    * when it receives a server-initiated DETACHED (#RTL13)
      * and channel is initialized (#RTL13)
        * [does nothing](./spec/acceptance/realtime/channel_spec.rb#L2017)
      * and channel is failed
        * [does nothing (#RTL13)](./spec/acceptance/realtime/channel_spec.rb#L2038)
      * and channel is attached
        * [reattaches immediately (#RTL13a)](./spec/acceptance/realtime/channel_spec.rb#L2054)
      * and channel is suspended
        * [reattaches immediately (#RTL13a)](./spec/acceptance/realtime/channel_spec.rb#L2070)
      * and channel is attaching
        * [will move to the SUSPENDED state and then attempt to ATTACH with the ATTACHING state (#RTL13b)](./spec/acceptance/realtime/channel_spec.rb#L2092)
    * when it receives an ERROR ProtocolMessage
      * [should transition to the failed state and the error_reason should be set (#RTL14)](./spec/acceptance/realtime/channel_spec.rb#L2140)

### Ably::Realtime::Channels
_(see [spec/acceptance/realtime/channels_spec.rb](./spec/acceptance/realtime/channels_spec.rb))_
  * using JSON protocol
    * using shortcut method #channel on the client object
      * behaves like a channel
        * [returns a channel object](./spec/acceptance/realtime/channels_spec.rb#L6)
        * [returns channel object and passes the provided options](./spec/acceptance/realtime/channels_spec.rb#L12)
    * using #get method on client#channels
      * behaves like a channel
        * [returns a channel object](./spec/acceptance/realtime/channels_spec.rb#L6)
        * [returns channel object and passes the provided options](./spec/acceptance/realtime/channels_spec.rb#L12)
    * accessing an existing channel object with different options
      * [overrides the existing channel options and returns the channel object](./spec/acceptance/realtime/channels_spec.rb#L41)
    * accessing an existing channel object without specifying any channel options
      * [returns the existing channel without modifying the channel options](./spec/acceptance/realtime/channels_spec.rb#L53)
    * using undocumented array accessor [] method on client#channels
      * behaves like a channel
        * [returns a channel object](./spec/acceptance/realtime/channels_spec.rb#L6)
        * [returns channel object and passes the provided options](./spec/acceptance/realtime/channels_spec.rb#L12)

### Ably::Realtime::Client
_(see [spec/acceptance/realtime/client_spec.rb](./spec/acceptance/realtime/client_spec.rb))_
  * using JSON protocol
    * initialization
      * basic auth
        * [is enabled by default with a provided :key option](./spec/acceptance/realtime/client_spec.rb#L18)
        * :tls option
          * set to false to force a plain-text connection
            * [fails to connect because a private key cannot be sent over a non-secure connection](./spec/acceptance/realtime/client_spec.rb#L31)
      * token auth
        * with TLS enabled
          * and a pre-generated Token provided with the :token option
            * [connects using token auth](./spec/acceptance/realtime/client_spec.rb#L52)
          * with valid :key and :use_token_auth option set to true
            * [automatically authorizes on connect and generates a token](./spec/acceptance/realtime/client_spec.rb#L65)
          * with client_id
            * [connects using token auth](./spec/acceptance/realtime/client_spec.rb#L78)
        * with TLS disabled
          * and a pre-generated Token provided with the :token option
            * [connects using token auth](./spec/acceptance/realtime/client_spec.rb#L52)
          * with valid :key and :use_token_auth option set to true
            * [automatically authorizes on connect and generates a token](./spec/acceptance/realtime/client_spec.rb#L65)
          * with client_id
            * [connects using token auth](./spec/acceptance/realtime/client_spec.rb#L78)
        * with a Proc for the :auth_callback option
          * [calls the Proc](./spec/acceptance/realtime/client_spec.rb#L103)
          * [uses the token request returned from the callback when requesting a new token](./spec/acceptance/realtime/client_spec.rb#L110)
          * when the returned token has a client_id
            * [sets Auth#client_id to the new token's client_id immediately when connecting](./spec/acceptance/realtime/client_spec.rb#L118)
            * [sets Client#client_id to the new token's client_id immediately when connecting](./spec/acceptance/realtime/client_spec.rb#L126)
          * with a wildcard client_id token
            * and an explicit client_id in ClientOptions
              * [allows uses the explicit client_id in the connection](./spec/acceptance/realtime/client_spec.rb#L144)
            * and client_id omitted in ClientOptions
              * [uses the token provided clientId in the connection](./spec/acceptance/realtime/client_spec.rb#L160)
        * with an invalid wildcard "*" :client_id
          * [raises an exception](./spec/acceptance/realtime/client_spec.rb#L176)
      * realtime connection settings
        * defaults
          * [disconnected_retry_timeout is 15s](./spec/acceptance/realtime/client_spec.rb#L185)
          * [suspended_retry_timeout is 30s](./spec/acceptance/realtime/client_spec.rb#L190)
        * overriden in ClientOptions
          * [disconnected_retry_timeout is updated](./spec/acceptance/realtime/client_spec.rb#L199)
          * [suspended_retry_timeout is updated](./spec/acceptance/realtime/client_spec.rb#L204)
    * #connection
      * [provides access to the Connection object](./spec/acceptance/realtime/client_spec.rb#L213)
    * #channels
      * [provides access to the Channels collection object](./spec/acceptance/realtime/client_spec.rb#L220)
    * #auth
      * [provides access to the Realtime::Auth object](./spec/acceptance/realtime/client_spec.rb#L227)
    * #request (#RSC19*)
      * get
        * [returns an HttpPaginatedResponse object](./spec/acceptance/realtime/client_spec.rb#L237)
        * 404 request to invalid URL
          * [returns an object with 404 status code and error message](./spec/acceptance/realtime/client_spec.rb#L246)
        * paged results
          * [provides paging](./spec/acceptance/realtime/client_spec.rb#L260)

### Ably::Realtime::Connection failures
_(see [spec/acceptance/realtime/connection_failures_spec.rb](./spec/acceptance/realtime/connection_failures_spec.rb))_
  * using JSON protocol
    * authentication failure
      * when API key is invalid
        * with invalid app part of the key
          * [enters the failed state and returns a not found error](./spec/acceptance/realtime/connection_failures_spec.rb#L29)
        * with invalid key name part of the key
          * [enters the failed state and returns an authorization error](./spec/acceptance/realtime/connection_failures_spec.rb#L44)
      * with auth_url
        * opening a new connection
          * request fails due to network failure
            * [the connection moves to the disconnected state and tries again, returning again to the disconnected state (#RSA4c, #RSA4c1, #RSA4c2)](./spec/acceptance/realtime/connection_failures_spec.rb#L62)
          * request fails due to invalid content
            * [the connection moves to the disconnected state and tries again, returning again to the disconnected state (#RSA4c, #RSA4c1, #RSA4c2)](./spec/acceptance/realtime/connection_failures_spec.rb#L92)
        * existing CONNECTED connection
          * authorize request failure leaves connection in existing condition
            * [the connection remains in the CONNECTED state and authorize fails (#RSA4c, #RSA4c1, #RSA4c3)](./spec/acceptance/realtime/connection_failures_spec.rb#L120)
      * with auth_callback
        * opening a new connection
          * when callback fails due to an exception
            * [the connection moves to the disconnected state and tries again, returning again to the disconnected state (#RSA4c, #RSA4c1, #RSA4c2)](./spec/acceptance/realtime/connection_failures_spec.rb#L148)
          * existing CONNECTED connection
            * when callback fails due to the request taking longer than realtime_request_timeout
              * [the authorization request fails as configured in the realtime_request_timeout (#RSA4c, #RSA4c1, #RSA4c3)](./spec/acceptance/realtime/connection_failures_spec.rb#L179)
    * automatic connection retry
      * with invalid WebSocket host
        * when disconnected
          * [enters the suspended state after multiple attempts to connect](./spec/acceptance/realtime/connection_failures_spec.rb#L244)
          * for the first time
            * [reattempts connection immediately and then waits disconnected_retry_timeout for a subsequent attempt](./spec/acceptance/realtime/connection_failures_spec.rb#L265)
          * #close
            * [transitions connection state to :closed](./spec/acceptance/realtime/connection_failures_spec.rb#L282)
        * when connection state is :suspended
          * [stays in the suspended state after any number of reconnection attempts](./spec/acceptance/realtime/connection_failures_spec.rb#L301)
          * for the first time
            * [waits suspended_retry_timeout before attempting to reconnect](./spec/acceptance/realtime/connection_failures_spec.rb#L324)
          * #close
            * [transitions connection state to :closed](./spec/acceptance/realtime/connection_failures_spec.rb#L346)
        * when connection state is :failed
          * #close
            * [will not transition state to :close and fails with an InvalidStateChange exception](./spec/acceptance/realtime/connection_failures_spec.rb#L365)
        * #error_reason
          * [contains the error when state is disconnected](./spec/acceptance/realtime/connection_failures_spec.rb#L386)
          * [contains the error when state is suspended](./spec/acceptance/realtime/connection_failures_spec.rb#L386)
          * [contains the error when state is failed](./spec/acceptance/realtime/connection_failures_spec.rb#L386)
          * [is reset to nil when :connected](./spec/acceptance/realtime/connection_failures_spec.rb#L400)
          * [is reset to nil when :closed](./spec/acceptance/realtime/connection_failures_spec.rb#L411)
      * #connect
        * connection opening times out
          * [attempts to reconnect](./spec/acceptance/realtime/connection_failures_spec.rb#L442)
          * when retry intervals are stubbed to attempt reconnection quickly
            * [never calls the provided success block](./spec/acceptance/realtime/connection_failures_spec.rb#L466)
    * connection resume
      * when DISCONNECTED ProtocolMessage received from the server
        * [reconnects automatically and immediately](./spec/acceptance/realtime/connection_failures_spec.rb#L497)
        * and subsequently fails to reconnect
          * [retries every 15 seconds](./spec/acceptance/realtime/connection_failures_spec.rb#L529)
      * when websocket transport is abruptly disconnected
        * [reconnects automatically](./spec/acceptance/realtime/connection_failures_spec.rb#L572)
        * hosts used
          * [reconnects with the default host](./spec/acceptance/realtime/connection_failures_spec.rb#L588)
      * after successfully reconnecting and resuming
        * [retains connection_id and updates the connection_key (#RTN15e, #RTN16d)](./spec/acceptance/realtime/connection_failures_spec.rb#L612)
        * [includes the error received in the connection state change from Ably but leaves the channels attached](./spec/acceptance/realtime/connection_failures_spec.rb#L627)
        * [retains channel subscription state](./spec/acceptance/realtime/connection_failures_spec.rb#L653)
        * [retains the client_serial (#RTN15c2, #RTN15c3)](./spec/acceptance/realtime/connection_failures_spec.rb#L714)
        * when messages were published whilst the client was disconnected
          * [receives the messages published whilst offline](./spec/acceptance/realtime/connection_failures_spec.rb#L681)
      * when failing to resume
        * because the connection_key is not or no longer valid
          * [updates the connection_id and connection_key](./spec/acceptance/realtime/connection_failures_spec.rb#L754)
          * [issue a reattach for all attached channels and fail all message awaiting an ACK (#RTN15c3)](./spec/acceptance/realtime/connection_failures_spec.rb#L769)
          * [issue a reattach for all attaching channels and fail all queued messages (#RTN15c3)](./spec/acceptance/realtime/connection_failures_spec.rb#L807)
          * [issue a attach for all suspended channels (#RTN15c3)](./spec/acceptance/realtime/connection_failures_spec.rb#L843)
          * [sets the error reason on each channel](./spec/acceptance/realtime/connection_failures_spec.rb#L881)
          * [resets the client_serial (#RTN15c3)](./spec/acceptance/realtime/connection_failures_spec.rb#L896)
        * as the DISCONNECTED window to resume has passed
          * [starts a new connection automatically and does not try and resume](./spec/acceptance/realtime/connection_failures_spec.rb#L933)
      * when an ERROR protocol message is received
        * whilst connecting
          * with a token error code in the range 40140 <= code < 40150 (#RTN14b)
            * [triggers a re-authentication](./spec/acceptance/realtime/connection_failures_spec.rb#L960)
          * with an error code indicating an error other than a token failure (#RTN14g, #RTN15i)
            * [causes the connection to fail](./spec/acceptance/realtime/connection_failures_spec.rb#L976)
          * with no error code indicating an error other than a token failure (#RTN14g, #RTN15i)
            * [causes the connection to fail](./spec/acceptance/realtime/connection_failures_spec.rb#L989)
        * whilst connected
          * with a token error code in the range 40140 <= code < 40150 (#RTN14b)
            * [triggers a re-authentication](./spec/acceptance/realtime/connection_failures_spec.rb#L960)
          * with an error code indicating an error other than a token failure (#RTN14g, #RTN15i)
            * [causes the connection to fail](./spec/acceptance/realtime/connection_failures_spec.rb#L976)
          * with no error code indicating an error other than a token failure (#RTN14g, #RTN15i)
            * [causes the connection to fail](./spec/acceptance/realtime/connection_failures_spec.rb#L989)
      * whilst resuming
        * with a token error code in the region 40140 <= code < 40150 (RTN15c5)
          * [triggers a re-authentication and then resumes the connection](./spec/acceptance/realtime/connection_failures_spec.rb#L1033)
      * with any other error (#RTN15c4)
        * [moves the connection to the failed state](./spec/acceptance/realtime/connection_failures_spec.rb#L1065)
    * fallback host feature
      * with custom realtime websocket host option
        * [never uses a fallback host](./spec/acceptance/realtime/connection_failures_spec.rb#L1109)
      * with custom realtime websocket port option
        * [never uses a fallback host](./spec/acceptance/realtime/connection_failures_spec.rb#L1127)
      * with non-production environment
        * [does not use a fallback host by default](./spec/acceptance/realtime/connection_failures_spec.rb#L1146)
        * :fallback_hosts_use_default is true
          * [uses a fallback host on every subsequent disconnected attempt until suspended (#RTN17b, #TO3k7)](./spec/acceptance/realtime/connection_failures_spec.rb#L1164)
        * :fallback_hosts array is provided
          * [uses a fallback host on every subsequent disconnected attempt until suspended (#RTN17b, #TO3k6)](./spec/acceptance/realtime/connection_failures_spec.rb#L1189)
      * with production environment
        * when the Internet is down
          * [never uses a fallback host](./spec/acceptance/realtime/connection_failures_spec.rb#L1225)
        * when the Internet is up
          * and default options
            * [uses a fallback host + the original host once on every subsequent disconnected attempt until suspended](./spec/acceptance/realtime/connection_failures_spec.rb#L1248)
            * [uses the primary host when suspended, and then every fallback host and the primary host again on every subsequent suspended attempt](./spec/acceptance/realtime/connection_failures_spec.rb#L1267)
          * :fallback_hosts array is provided by an empty array
            * [uses a fallback host on every subsequent disconnected attempt until suspended (#RTN17b, #TO3k6)](./spec/acceptance/realtime/connection_failures_spec.rb#L1297)
          * :fallback_hosts array is provided
            * [uses a fallback host on every subsequent disconnected attempt until suspended (#RTN17b, #TO3k6)](./spec/acceptance/realtime/connection_failures_spec.rb#L1317)

### Ably::Realtime::Connection
_(see [spec/acceptance/realtime/connection_spec.rb](./spec/acceptance/realtime/connection_spec.rb))_
  * using JSON protocol
    * intialization
      * [connects automatically](./spec/acceptance/realtime/connection_spec.rb#L23)
      * with :auto_connect option set to false
        * [does not connect automatically](./spec/acceptance/realtime/connection_spec.rb#L35)
        * [connects when method #connect is called](./spec/acceptance/realtime/connection_spec.rb#L43)
      * with token auth
        * for renewable tokens
          * that are valid for the duration of the test
            * with valid pre authorized token expiring in the future
              * [uses the existing token created by Auth](./spec/acceptance/realtime/connection_spec.rb#L65)
            * with implicit authorisation
              * [uses the token created by the implicit authorisation](./spec/acceptance/realtime/connection_spec.rb#L77)
          * that expire
            * opening a new connection
              * with almost expired tokens
                * [renews token every time after it expires](./spec/acceptance/realtime/connection_spec.rb#L111)
              * with immediately expired token
                * [renews the token on connect, and makes one immediate subsequent attempt to obtain a new token (#RSA4b)](./spec/acceptance/realtime/connection_spec.rb#L141)
                * when disconnected_retry_timeout is 0.5 seconds
                  * [renews the token on connect, and continues to attempt renew based on the retry schedule](./spec/acceptance/realtime/connection_spec.rb#L156)
                * using implicit token auth
                  * [uses the primary host for subsequent connection and auth requests](./spec/acceptance/realtime/connection_spec.rb#L186)
            * when connected with a valid non-expired token
              * that then expires following the connection being opened
                * the server
                  * [disconnects the client, and the client automatically renews the token and then reconnects](./spec/acceptance/realtime/connection_spec.rb#L213)
                * connection state
                  * PENDING: *[retains messages published when disconnected three times during authentication](./spec/acceptance/realtime/connection_spec.rb#L273)*
                * and subsequent token is invalid
                  * [transitions the connection to the failed state](./spec/acceptance/realtime/connection_spec.rb#L309)
        * for non-renewable tokens
          * that are expired
            * opening a new connection
              * [transitions state to failed (#RSA4a)](./spec/acceptance/realtime/connection_spec.rb#L339)
            * when connected
              * [transitions state to failed (#RSA4a)](./spec/acceptance/realtime/connection_spec.rb#L355)
        * with opaque token string that contain an implicit client_id
          * string
            * [sets the Client#client_id and Auth#client_id once CONNECTED](./spec/acceptance/realtime/connection_spec.rb#L375)
            * that is incompatible with the current client client_id
              * [fails the connection](./spec/acceptance/realtime/connection_spec.rb#L387)
          * wildcard
            * [configures the Client#client_id and Auth#client_id with a wildcard once CONNECTED](./spec/acceptance/realtime/connection_spec.rb#L401)
    * initialization state changes
      * with implicit #connect
        * [are emitted in order](./spec/acceptance/realtime/connection_spec.rb#L433)
      * with explicit #connect
        * [are emitted in order](./spec/acceptance/realtime/connection_spec.rb#L439)
    * #connect
      * [returns a SafeDeferrable that catches exceptions in callbacks and logs them](./spec/acceptance/realtime/connection_spec.rb#L447)
      * [calls the Deferrable callback on success](./spec/acceptance/realtime/connection_spec.rb#L452)
      * [calls the provided block on success even if state changes to disconnected first](./spec/acceptance/realtime/connection_spec.rb#L459)
      * with invalid auth details
        * [calls the Deferrable errback only once on connection failure](./spec/acceptance/realtime/connection_spec.rb#L488)
      * when already connected
        * [does nothing and no further state changes are emitted](./spec/acceptance/realtime/connection_spec.rb#L504)
      * connection#id
        * [is null before connecting](./spec/acceptance/realtime/connection_spec.rb#L518)
      * connection#key
        * [is null before connecting](./spec/acceptance/realtime/connection_spec.rb#L525)
      * once connected
        * connection#id
          * [is a string](./spec/acceptance/realtime/connection_spec.rb#L536)
          * [is unique from the connection#key](./spec/acceptance/realtime/connection_spec.rb#L543)
          * [is unique for every connection](./spec/acceptance/realtime/connection_spec.rb#L550)
        * connection#key
          * [is a string](./spec/acceptance/realtime/connection_spec.rb#L559)
          * [is unique from the connection#id](./spec/acceptance/realtime/connection_spec.rb#L566)
          * [is unique for every connection](./spec/acceptance/realtime/connection_spec.rb#L573)
      * following a previous connection being opened and closed
        * [reconnects and is provided with a new connection ID and connection key from the server](./spec/acceptance/realtime/connection_spec.rb#L583)
      * when closing
        * [fails the deferrable before the connection is closed](./spec/acceptance/realtime/connection_spec.rb#L600)
    * #serial connection serial
      * [is set to -1 when a new connection is opened](./spec/acceptance/realtime/connection_spec.rb#L617)
      * [is set to 0 when a message sent ACK is received](./spec/acceptance/realtime/connection_spec.rb#L640)
      * [is set to 1 when the second message sent ACK is received](./spec/acceptance/realtime/connection_spec.rb#L647)
      * when a message is sent but the ACK has not yet been received
        * [the sent message msgSerial is 0 but the connection serial remains at -1](./spec/acceptance/realtime/connection_spec.rb#L625)
    * #close
      * [returns a SafeDeferrable that catches exceptions in callbacks and logs them](./spec/acceptance/realtime/connection_spec.rb#L658)
      * [calls the Deferrable callback on success](./spec/acceptance/realtime/connection_spec.rb#L665)
      * when already closed
        * [does nothing and no further state changes are emitted](./spec/acceptance/realtime/connection_spec.rb#L676)
      * when connection state is
        * :initialized
          * [changes the connection state to :closing and then immediately :closed without sending a ProtocolMessage CLOSE](./spec/acceptance/realtime/connection_spec.rb#L703)
        * :connected
          * [changes the connection state to :closing and waits for the server to confirm connection is :closed with a ProtocolMessage](./spec/acceptance/realtime/connection_spec.rb#L720)
          * with an unresponsive connection
            * [force closes the connection when a :closed ProtocolMessage response is not received](./spec/acceptance/realtime/connection_spec.rb#L747)
    * #ping
      * [echoes a heart beat (#RTN13a)](./spec/acceptance/realtime/connection_spec.rb#L769)
      * [sends a unique ID in each protocol message (#RTN13e)](./spec/acceptance/realtime/connection_spec.rb#L779)
      * [waits until the connection becomes CONNECTED when in the CONNETING state](./spec/acceptance/realtime/connection_spec.rb#L803)
      * with incompatible states
        * when not connected
          * [fails the deferrable (#RTN13b)](./spec/acceptance/realtime/connection_spec.rb#L816)
        * when suspended
          * [fails the deferrable (#RTN13b)](./spec/acceptance/realtime/connection_spec.rb#L825)
        * when failed
          * [fails the deferrable (#RTN13b)](./spec/acceptance/realtime/connection_spec.rb#L837)
        * when closed
          * [fails the deferrable (#RTN13b)](./spec/acceptance/realtime/connection_spec.rb#L849)
        * when it becomes closed
          * [fails the deferrable (#RTN13b)](./spec/acceptance/realtime/connection_spec.rb#L863)
      * with a success block that raises an exception
        * [catches the exception and logs the error](./spec/acceptance/realtime/connection_spec.rb#L876)
      * when ping times out
        * [fails the deferrable logs a warning (#RTN13a, #RTN13c)](./spec/acceptance/realtime/connection_spec.rb#L890)
        * [yields to the block with a nil value](./spec/acceptance/realtime/connection_spec.rb#L909)
    * Heartbeats (#RTN23)
      * heartbeat interval
        * when reduced artificially
          * [is the sum of the max_idle_interval and realtime_request_timeout (#RTN23a)](./spec/acceptance/realtime/connection_spec.rb#L936)
          * [disconnects the transport if no heartbeat received since connected (#RTN23a)](./spec/acceptance/realtime/connection_spec.rb#L946)
          * [disconnects the transport if no heartbeat received since last event received (#RTN23a)](./spec/acceptance/realtime/connection_spec.rb#L957)
      * transport-level heartbeats are supported in the websocket transport
        * [provides the heartbeats argument in the websocket connection params (#RTN23b)](./spec/acceptance/realtime/connection_spec.rb#L972)
        * PENDING: *[receives websocket heartbeat messages (#RTN23b) [slow test as need to wait for heartbeat]](./spec/acceptance/realtime/connection_spec.rb#L981)*
      * with websocket heartbeats disabled (undocumented)
        * [does not provide the heartbeats argument in the websocket connection params (#RTN23b)](./spec/acceptance/realtime/connection_spec.rb#L1000)
        * [receives websocket protocol messages (#RTN23b) [slow test as need to wait for heartbeat]](./spec/acceptance/realtime/connection_spec.rb#L1009)
    * #details
      * [is nil before connected](./spec/acceptance/realtime/connection_spec.rb#L1026)
      * [contains the ConnectionDetails object once connected (#RTN21)](./spec/acceptance/realtime/connection_spec.rb#L1033)
      * [contains the new ConnectionDetails object once a subsequent connection is created (#RTN21)](./spec/acceptance/realtime/connection_spec.rb#L1042)
      * with a different default connection_state_ttl
        * [updates the private Connection#connection_state_ttl when received from Ably in ConnectionDetails](./spec/acceptance/realtime/connection_spec.rb#L1063)
    * recovery
      * #recovery_key
        * [is composed of connection key and serial that is kept up to date with each message ACK received](./spec/acceptance/realtime/connection_spec.rb#L1100)
        * [is available when connection is in one of the states: connecting, connected, disconnected](./spec/acceptance/realtime/connection_spec.rb#L1123)
        * [is nil when connection is explicitly CLOSED](./spec/acceptance/realtime/connection_spec.rb#L1152)
      * opening a new connection using a recently disconnected connection's #recovery_key
        * connection#id and connection#key after recovery
          * [remains the same for id and party for key](./spec/acceptance/realtime/connection_spec.rb#L1164)
        * when messages have been sent whilst the old connection is disconnected
          * the new connection
            * [recovers server-side queued messages](./spec/acceptance/realtime/connection_spec.rb#L1206)
      * with :recover option
        * with invalid syntax
          * [raises an exception](./spec/acceptance/realtime/connection_spec.rb#L1238)
        * with invalid formatted value sent to server
          * [sets the #error_reason and moves the connection to FAILED](./spec/acceptance/realtime/connection_spec.rb#L1247)
        * with expired (missing) value sent to server
          * [connects but sets the error reason and includes the reason in the state change](./spec/acceptance/realtime/connection_spec.rb#L1262)
    * with many connections simultaneously
      * [opens each with a unique connection#id and connection#key](./spec/acceptance/realtime/connection_spec.rb#L1281)
    * when a state transition is unsupported
      * [logs the invalid state change as fatal](./spec/acceptance/realtime/connection_spec.rb#L1301)
    * protocol failure
      * receiving an invalid ProtocolMessage
        * [emits an error on the connection and logs a fatal error message](./spec/acceptance/realtime/connection_spec.rb#L1317)
    * undocumented method
      * #internet_up?
        * [returns a Deferrable](./spec/acceptance/realtime/connection_spec.rb#L1335)
        * internet up URL protocol
          * when using TLS for the connection
            * [uses TLS for the Internet check to https://internet-up.ably-realtime.com/is-the-internet-up.txt](./spec/acceptance/realtime/connection_spec.rb#L1346)
          * when using a non-secured connection
            * [uses TLS for the Internet check to http://internet-up.ably-realtime.com/is-the-internet-up.txt](./spec/acceptance/realtime/connection_spec.rb#L1356)
        * when the Internet is up
          * [calls the block with true](./spec/acceptance/realtime/connection_spec.rb#L1387)
          * [calls the success callback of the Deferrable](./spec/acceptance/realtime/connection_spec.rb#L1394)
          * with a TLS connection
            * [checks the Internet up URL over TLS](./spec/acceptance/realtime/connection_spec.rb#L1370)
          * with a non-TLS connection
            * [checks the Internet up URL over TLS](./spec/acceptance/realtime/connection_spec.rb#L1380)
        * when the Internet is down
          * [calls the block with false](./spec/acceptance/realtime/connection_spec.rb#L1409)
          * [calls the failure callback of the Deferrable](./spec/acceptance/realtime/connection_spec.rb#L1416)
    * state change side effects
      * when connection enters the :disconnected state
        * [queues messages to be sent and all channels remain attached](./spec/acceptance/realtime/connection_spec.rb#L1430)
      * when connection enters the :suspended state
        * [moves the channels into the suspended state and prevents publishing of messages on those channels](./spec/acceptance/realtime/connection_spec.rb#L1463)
      * when connection enters the :failed state
        * [sets all channels to failed and prevents publishing of messages on those channels](./spec/acceptance/realtime/connection_spec.rb#L1494)
    * connection state change
      * [emits event to all and single subscribers](./spec/acceptance/realtime/connection_spec.rb#L1508)
      * [emits a ConnectionStateChange object](./spec/acceptance/realtime/connection_spec.rb#L1523)
      * ConnectionStateChange object
        * [has current state](./spec/acceptance/realtime/connection_spec.rb#L1531)
        * [has a previous state](./spec/acceptance/realtime/connection_spec.rb#L1539)
        * [has the event that generated the state change (#TH5)](./spec/acceptance/realtime/connection_spec.rb#L1547)
        * [has an empty reason when there is no error](./spec/acceptance/realtime/connection_spec.rb#L1563)
        * on failure
          * [has a reason Error object when there is an error on the connection](./spec/acceptance/realtime/connection_spec.rb#L1576)
        * retry_in
          * [is nil when a retry is not required](./spec/acceptance/realtime/connection_spec.rb#L1591)
          * [is 0 when first attempt to connect fails](./spec/acceptance/realtime/connection_spec.rb#L1598)
          * [is 0 when an immediate reconnect will occur](./spec/acceptance/realtime/connection_spec.rb#L1608)
          * [contains the next retry period when an immediate reconnect will not occur](./spec/acceptance/realtime/connection_spec.rb#L1618)
      * whilst CONNECTED
        * when a CONNECTED message is received (#RTN24)
          * [emits an UPDATE event](./spec/acceptance/realtime/connection_spec.rb#L1653)
          * [updates the ConnectionDetail and Connection attributes (#RTC8a1)](./spec/acceptance/realtime/connection_spec.rb#L1668)
        * when a CONNECTED message with an error is received
          * [emits an UPDATE event](./spec/acceptance/realtime/connection_spec.rb#L1703)
    * version params
      * [sends the protocol version param v (#G4, #RTN2f)](./spec/acceptance/realtime/connection_spec.rb#L1724)
      * [sends the lib version param lib (#RTN2g)](./spec/acceptance/realtime/connection_spec.rb#L1733)
      * with variant
        * [sends the lib version param lib with the variant (#RTN2g + #RSC7b)](./spec/acceptance/realtime/connection_spec.rb#L1753)

### Ably::Realtime::Channel Message
_(see [spec/acceptance/realtime/message_spec.rb](./spec/acceptance/realtime/message_spec.rb))_
  * using JSON protocol
    * [sends a String data payload](./spec/acceptance/realtime/message_spec.rb#L25)
    * with supported data payload content type
      * JSON Object (Hash)
        * [is encoded and decoded to the same hash](./spec/acceptance/realtime/message_spec.rb#L48)
      * JSON Array
        * [is encoded and decoded to the same Array](./spec/acceptance/realtime/message_spec.rb#L56)
      * String
        * [is encoded and decoded to the same Array](./spec/acceptance/realtime/message_spec.rb#L64)
      * Binary
        * [is encoded and decoded to the same Array](./spec/acceptance/realtime/message_spec.rb#L72)
    * with supported extra payload content type (#RTL6h, #RSL6a2)
      * JSON Object (Hash)
        * PENDING: *[is encoded and decoded to the same hash](./spec/acceptance/realtime/message_spec.rb#L91)*
      * JSON Array
        * PENDING: *[is encoded and decoded to the same Array](./spec/acceptance/realtime/message_spec.rb#L100)*
      * nil
        * [is encoded and decoded to the same Array](./spec/acceptance/realtime/message_spec.rb#L107)
    * with unsupported data payload content type
      * Integer
        * [is raises an UnsupportedDataType 40011 exception](./spec/acceptance/realtime/message_spec.rb#L118)
      * Float
        * [is raises an UnsupportedDataType 40011 exception](./spec/acceptance/realtime/message_spec.rb#L127)
      * Boolean
        * [is raises an UnsupportedDataType 40011 exception](./spec/acceptance/realtime/message_spec.rb#L136)
      * False
        * [is raises an UnsupportedDataType 40011 exception](./spec/acceptance/realtime/message_spec.rb#L145)
    * with ASCII_8BIT message name
      * [is converted into UTF_8](./spec/acceptance/realtime/message_spec.rb#L154)
    * when the message publisher has a client_id
      * [contains a #client_id attribute](./spec/acceptance/realtime/message_spec.rb#L170)
    * #connection_id attribute
      * over realtime
        * [matches the sender connection#id](./spec/acceptance/realtime/message_spec.rb#L183)
      * when retrieved over REST
        * [matches the sender connection#id](./spec/acceptance/realtime/message_spec.rb#L195)
    * local echo when published
      * [is enabled by default](./spec/acceptance/realtime/message_spec.rb#L207)
      * with :echo_messages option set to false
        * [will not echo messages to the client but will still broadcast messages to other connected clients](./spec/acceptance/realtime/message_spec.rb#L227)
        * [will not echo messages to the client from other REST clients publishing using that connection_key](./spec/acceptance/realtime/message_spec.rb#L246)
        * [will echo messages with a valid connection_id to the client from other REST clients publishing using that connection_key](./spec/acceptance/realtime/message_spec.rb#L259)
    * publishing lots of messages across two connections
      * [sends and receives the messages on both opened connections and calls the success callbacks for each message published](./spec/acceptance/realtime/message_spec.rb#L285)
    * without suitable publishing permissions
      * [calls the error callback](./spec/acceptance/realtime/message_spec.rb#L330)
    * server incorrectly resends a message that was already received by the client library
      * [discards the message and logs it as an error to the channel](./spec/acceptance/realtime/message_spec.rb#L349)
    * encoding and decoding encrypted messages
      * with AES-128-CBC using crypto-data-128.json fixtures (#RTL7d)
        * item 0 with encrypted encoding utf-8/cipher+aes-128-cbc/base64
          * behaves like an Ably encrypter and decrypter
            * with #publish and #subscribe
              * [encrypts message automatically before they are pushed to the server (#RTL7d)](./spec/acceptance/realtime/message_spec.rb#L414)
              * [sends and receives messages that are encrypted & decrypted by the Ably library (#RTL7d)](./spec/acceptance/realtime/message_spec.rb#L432)
        * item 1 with encrypted encoding cipher+aes-128-cbc/base64
          * behaves like an Ably encrypter and decrypter
            * with #publish and #subscribe
              * [encrypts message automatically before they are pushed to the server (#RTL7d)](./spec/acceptance/realtime/message_spec.rb#L414)
              * [sends and receives messages that are encrypted & decrypted by the Ably library (#RTL7d)](./spec/acceptance/realtime/message_spec.rb#L432)
        * item 2 with encrypted encoding json/utf-8/cipher+aes-128-cbc/base64
          * behaves like an Ably encrypter and decrypter
            * with #publish and #subscribe
              * [encrypts message automatically before they are pushed to the server (#RTL7d)](./spec/acceptance/realtime/message_spec.rb#L414)
              * [sends and receives messages that are encrypted & decrypted by the Ably library (#RTL7d)](./spec/acceptance/realtime/message_spec.rb#L432)
        * item 3 with encrypted encoding json/utf-8/cipher+aes-128-cbc/base64
          * behaves like an Ably encrypter and decrypter
            * with #publish and #subscribe
              * [encrypts message automatically before they are pushed to the server (#RTL7d)](./spec/acceptance/realtime/message_spec.rb#L414)
              * [sends and receives messages that are encrypted & decrypted by the Ably library (#RTL7d)](./spec/acceptance/realtime/message_spec.rb#L432)
      * with AES-256-CBC using crypto-data-256.json fixtures (#RTL7d)
        * item 0 with encrypted encoding utf-8/cipher+aes-256-cbc/base64
          * behaves like an Ably encrypter and decrypter
            * with #publish and #subscribe
              * [encrypts message automatically before they are pushed to the server (#RTL7d)](./spec/acceptance/realtime/message_spec.rb#L414)
              * [sends and receives messages that are encrypted & decrypted by the Ably library (#RTL7d)](./spec/acceptance/realtime/message_spec.rb#L432)
        * item 1 with encrypted encoding cipher+aes-256-cbc/base64
          * behaves like an Ably encrypter and decrypter
            * with #publish and #subscribe
              * [encrypts message automatically before they are pushed to the server (#RTL7d)](./spec/acceptance/realtime/message_spec.rb#L414)
              * [sends and receives messages that are encrypted & decrypted by the Ably library (#RTL7d)](./spec/acceptance/realtime/message_spec.rb#L432)
        * item 2 with encrypted encoding json/utf-8/cipher+aes-256-cbc/base64
          * behaves like an Ably encrypter and decrypter
            * with #publish and #subscribe
              * [encrypts message automatically before they are pushed to the server (#RTL7d)](./spec/acceptance/realtime/message_spec.rb#L414)
              * [sends and receives messages that are encrypted & decrypted by the Ably library (#RTL7d)](./spec/acceptance/realtime/message_spec.rb#L432)
        * item 3 with encrypted encoding json/utf-8/cipher+aes-256-cbc/base64
          * behaves like an Ably encrypter and decrypter
            * with #publish and #subscribe
              * [encrypts message automatically before they are pushed to the server (#RTL7d)](./spec/acceptance/realtime/message_spec.rb#L414)
              * [sends and receives messages that are encrypted & decrypted by the Ably library (#RTL7d)](./spec/acceptance/realtime/message_spec.rb#L432)
      * with multiple sends from one client to another
        * [encrypts and decrypts all messages](./spec/acceptance/realtime/message_spec.rb#L471)
        * [receives raw messages with the correct encoding](./spec/acceptance/realtime/message_spec.rb#L488)
      * subscribing with a different transport protocol
        * [delivers a String ASCII-8BIT payload to the receiver](./spec/acceptance/realtime/message_spec.rb#L522)
        * [delivers a String UTF-8 payload to the receiver](./spec/acceptance/realtime/message_spec.rb#L522)
        * [delivers a Hash payload to the receiver](./spec/acceptance/realtime/message_spec.rb#L522)
      * publishing on an unencrypted channel and subscribing on an encrypted channel with another client
        * [does not attempt to decrypt the message](./spec/acceptance/realtime/message_spec.rb#L543)
      * publishing on an encrypted channel and subscribing on an unencrypted channel with another client
        * [delivers the message but still encrypted with a value in the #encoding attribute (#RTL7e)](./spec/acceptance/realtime/message_spec.rb#L561)
        * [logs a Cipher error (#RTL7e)](./spec/acceptance/realtime/message_spec.rb#L572)
      * publishing on an encrypted channel and subscribing with a different algorithm on another client
        * [delivers the message but still encrypted with the cipher detials in the #encoding attribute (#RTL7e)](./spec/acceptance/realtime/message_spec.rb#L592)
        * [emits a Cipher error on the channel (#RTL7e)](./spec/acceptance/realtime/message_spec.rb#L601)
      * publishing on an encrypted channel and subscribing with a different key on another client
        * [delivers the message but still encrypted with the cipher details in the #encoding attribute](./spec/acceptance/realtime/message_spec.rb#L621)
        * [emits a Cipher error on the channel](./spec/acceptance/realtime/message_spec.rb#L632)
    * when message is published, the connection disconnects before the ACK is received, and the connection is resumed
      * [publishes the message again, later receives the ACK and only one message is ever received from Ably](./spec/acceptance/realtime/message_spec.rb#L651)
    * when message is published, the connection disconnects before the ACK is received
      * the connection is not resumed
        * [calls the errback for all messages](./spec/acceptance/realtime/message_spec.rb#L694)
      * the connection becomes suspended
        * [calls the errback for all messages](./spec/acceptance/realtime/message_spec.rb#L720)
      * the connection becomes failed
        * [calls the errback for all messages](./spec/acceptance/realtime/message_spec.rb#L747)
  * message encoding interoperability
    * over a JSON transport
      * when decoding string
        * [ensures that client libraries have compatible encoding and decoding using common fixtures](./spec/acceptance/realtime/message_spec.rb#L788)
      * when encoding string
        * [ensures that client libraries have compatible encoding and decoding using common fixtures](./spec/acceptance/realtime/message_spec.rb#L806)
      * when decoding string
        * [ensures that client libraries have compatible encoding and decoding using common fixtures](./spec/acceptance/realtime/message_spec.rb#L788)
      * when encoding string
        * [ensures that client libraries have compatible encoding and decoding using common fixtures](./spec/acceptance/realtime/message_spec.rb#L806)
      * when decoding jsonObject
        * [ensures that client libraries have compatible encoding and decoding using common fixtures](./spec/acceptance/realtime/message_spec.rb#L788)
      * when encoding jsonObject
        * [ensures that client libraries have compatible encoding and decoding using common fixtures](./spec/acceptance/realtime/message_spec.rb#L806)
      * when decoding jsonArray
        * [ensures that client libraries have compatible encoding and decoding using common fixtures](./spec/acceptance/realtime/message_spec.rb#L788)
      * when encoding jsonArray
        * [ensures that client libraries have compatible encoding and decoding using common fixtures](./spec/acceptance/realtime/message_spec.rb#L806)
      * when decoding binary
        * [ensures that client libraries have compatible encoding and decoding using common fixtures](./spec/acceptance/realtime/message_spec.rb#L788)
      * when encoding binary
        * [ensures that client libraries have compatible encoding and decoding using common fixtures](./spec/acceptance/realtime/message_spec.rb#L806)
    * over a MsgPack transport
      * when publishing a string using JSON protocol
        * [receives the message over MsgPack and the data matches](./spec/acceptance/realtime/message_spec.rb#L840)
      * when retrieving a string using JSON protocol
        * [is compatible with a publishes using MsgPack](./spec/acceptance/realtime/message_spec.rb#L868)
      * when publishing a string using JSON protocol
        * [receives the message over MsgPack and the data matches](./spec/acceptance/realtime/message_spec.rb#L840)
      * when retrieving a string using JSON protocol
        * [is compatible with a publishes using MsgPack](./spec/acceptance/realtime/message_spec.rb#L868)
      * when publishing a jsonObject using JSON protocol
        * [receives the message over MsgPack and the data matches](./spec/acceptance/realtime/message_spec.rb#L840)
      * when retrieving a jsonObject using JSON protocol
        * [is compatible with a publishes using MsgPack](./spec/acceptance/realtime/message_spec.rb#L868)
      * when publishing a jsonArray using JSON protocol
        * [receives the message over MsgPack and the data matches](./spec/acceptance/realtime/message_spec.rb#L840)
      * when retrieving a jsonArray using JSON protocol
        * [is compatible with a publishes using MsgPack](./spec/acceptance/realtime/message_spec.rb#L868)
      * when publishing a binary using JSON protocol
        * [receives the message over MsgPack and the data matches](./spec/acceptance/realtime/message_spec.rb#L840)
      * when retrieving a binary using JSON protocol
        * [is compatible with a publishes using MsgPack](./spec/acceptance/realtime/message_spec.rb#L868)

### Ably::Realtime::Presence history
_(see [spec/acceptance/realtime/presence_history_spec.rb](./spec/acceptance/realtime/presence_history_spec.rb))_
  * using JSON protocol
    * [provides up to the moment presence history](./spec/acceptance/realtime/presence_history_spec.rb#L21)
    * [ensures REST presence history message IDs match ProtocolMessage wrapped message and connection IDs via Realtime](./spec/acceptance/realtime/presence_history_spec.rb#L42)
    * with option until_attach: true
      * [retrieves all presence messages before channel was attached](./spec/acceptance/realtime/presence_history_spec.rb#L61)
      * [fails with an exception unless state is attached](./spec/acceptance/realtime/presence_history_spec.rb#L97)
      * and two pages of messages
        * [retrieves two pages of messages before channel was attached](./spec/acceptance/realtime/presence_history_spec.rb#L78)

### Ably::Realtime::Presence
_(see [spec/acceptance/realtime/presence_spec.rb](./spec/acceptance/realtime/presence_spec.rb))_
  * using JSON protocol
    * when attached (but not present) on a presence channel with an anonymous client (no client ID)
      * [maintains state as other clients enter and leave the channel (#RTP2e)](./spec/acceptance/realtime/presence_spec.rb#L479)
    * #sync_complete? and SYNC flags (#RTP1)
      * when attaching to a channel without any members present
        * [sync_complete? is true, there is no presence flag, and the presence channel is considered synced immediately (#RTP1)](./spec/acceptance/realtime/presence_spec.rb#L701)
      * when attaching to a channel with members present
        * [sync_complete? is false, there is a presence flag, and the presence channel is subsequently synced (#RTP1)](./spec/acceptance/realtime/presence_spec.rb#L722)
    * 101 existing (present) members on a channel (2 SYNC pages)
      * requiring at least 2 SYNC ProtocolMessages
        * when a client attaches to the presence channel
          * [emits :present for each member](./spec/acceptance/realtime/presence_spec.rb#L771)
          * and a member enters before the SYNC operation is complete
            * [emits a :enter immediately and the member is :present once the sync is complete (#RTP2g)](./spec/acceptance/realtime/presence_spec.rb#L787)
          * and a member leaves before the SYNC operation is complete
            * [emits :leave immediately as the member leaves and cleans up the ABSENT member after (#RTP2f, #RTP2g)](./spec/acceptance/realtime/presence_spec.rb#L824)
            * [ignores presence events with timestamps / identifiers prior to the current :present event in the MembersMap (#RTP2c)](./spec/acceptance/realtime/presence_spec.rb#L872)
            * [does not emit :present after the :leave event has been emitted, and that member is not included in the list of members via #get (#RTP2f)](./spec/acceptance/realtime/presence_spec.rb#L917)
          * #get
            * by default
              * [waits until sync is complete (#RTP11c1)](./spec/acceptance/realtime/presence_spec.rb#L967)
            * with :wait_for_sync option set to false (#RTP11c1)
              * [it does not wait for sync](./spec/acceptance/realtime/presence_spec.rb#L986)
    * state
      * once opened
        * [once opened, enters the :left state if the channel detaches](./spec/acceptance/realtime/presence_spec.rb#L1012)
    * #enter
      * data attribute
        * when provided as argument option to #enter
          * [changes to value provided in #leave](./spec/acceptance/realtime/presence_spec.rb#L1037)
      * message #connection_id
        * [matches the current client connection_id](./spec/acceptance/realtime/presence_spec.rb#L1061)
      * without necessary capabilities to join presence
        * [calls the Deferrable errback on capabilities failure](./spec/acceptance/realtime/presence_spec.rb#L1080)
      * it should behave like a public presence method
        * [raise an exception if the channel is detached](./spec/acceptance/realtime/presence_spec.rb#L63)
        * [raise an exception if the channel becomes detached](./spec/acceptance/realtime/presence_spec.rb#L81)
        * [raise an exception if the channel is failed](./spec/acceptance/realtime/presence_spec.rb#L97)
        * [raise an exception if the channel becomes failed](./spec/acceptance/realtime/presence_spec.rb#L114)
        * [implicitly attaches the channel](./spec/acceptance/realtime/presence_spec.rb#L130)
        * [returns a SafeDeferrable that catches exceptions in callbacks and logs them](./spec/acceptance/realtime/presence_spec.rb#L293)
        * [allows a block to be passed in that is executed upon success](./spec/acceptance/realtime/presence_spec.rb#L300)
        * [calls the Deferrable callback on success](./spec/acceptance/realtime/presence_spec.rb#L308)
        * [catches exceptions in the provided method block and logs them to the logger](./spec/acceptance/realtime/presence_spec.rb#L318)
        * when :queue_messages client option is false
          * and connection state initialized
            * [fails the deferrable](./spec/acceptance/realtime/presence_spec.rb#L142)
          * and connection state connecting
            * [fails the deferrable](./spec/acceptance/realtime/presence_spec.rb#L152)
          * and connection state disconnected
            * [fails the deferrable](./spec/acceptance/realtime/presence_spec.rb#L167)
          * and connection state connected
            * [publishes the message](./spec/acceptance/realtime/presence_spec.rb#L182)
        * with supported data payload content type
          * JSON Object (Hash)
            * [is encoded and decoded to the same hash](./spec/acceptance/realtime/presence_spec.rb#L209)
          * JSON Array
            * [is encoded and decoded to the same Array](./spec/acceptance/realtime/presence_spec.rb#L219)
          * String
            * [is encoded and decoded to the same Array](./spec/acceptance/realtime/presence_spec.rb#L229)
          * Binary
            * [is encoded and decoded to the same Array](./spec/acceptance/realtime/presence_spec.rb#L239)
        * with unsupported data payload content type
          * Integer
            * [raises an UnsupportedDataType 40011 exception](./spec/acceptance/realtime/presence_spec.rb#L259)
          * Float
            * [raises an UnsupportedDataType 40011 exception](./spec/acceptance/realtime/presence_spec.rb#L268)
          * Boolean
            * [raises an UnsupportedDataType 40011 exception](./spec/acceptance/realtime/presence_spec.rb#L277)
          * False
            * [raises an UnsupportedDataType 40011 exception](./spec/acceptance/realtime/presence_spec.rb#L286)
        * if connection fails before success
          * [calls the Deferrable errback if channel is detached](./spec/acceptance/realtime/presence_spec.rb#L331)
    * #update
      * [without previous #enter automatically enters](./spec/acceptance/realtime/presence_spec.rb#L1092)
      * [updates the data if :data argument provided](./spec/acceptance/realtime/presence_spec.rb#L1117)
      * [updates the data to nil if :data argument is not provided (assumes nil value)](./spec/acceptance/realtime/presence_spec.rb#L1127)
      * when ENTERED
        * [has no effect on the state](./spec/acceptance/realtime/presence_spec.rb#L1102)
      * it should behave like a public presence method
        * [raise an exception if the channel is detached](./spec/acceptance/realtime/presence_spec.rb#L63)
        * [raise an exception if the channel becomes detached](./spec/acceptance/realtime/presence_spec.rb#L81)
        * [raise an exception if the channel is failed](./spec/acceptance/realtime/presence_spec.rb#L97)
        * [raise an exception if the channel becomes failed](./spec/acceptance/realtime/presence_spec.rb#L114)
        * [implicitly attaches the channel](./spec/acceptance/realtime/presence_spec.rb#L130)
        * [returns a SafeDeferrable that catches exceptions in callbacks and logs them](./spec/acceptance/realtime/presence_spec.rb#L293)
        * [allows a block to be passed in that is executed upon success](./spec/acceptance/realtime/presence_spec.rb#L300)
        * [calls the Deferrable callback on success](./spec/acceptance/realtime/presence_spec.rb#L308)
        * [catches exceptions in the provided method block and logs them to the logger](./spec/acceptance/realtime/presence_spec.rb#L318)
        * when :queue_messages client option is false
          * and connection state initialized
            * [fails the deferrable](./spec/acceptance/realtime/presence_spec.rb#L142)
          * and connection state connecting
            * [fails the deferrable](./spec/acceptance/realtime/presence_spec.rb#L152)
          * and connection state disconnected
            * [fails the deferrable](./spec/acceptance/realtime/presence_spec.rb#L167)
          * and connection state connected
            * [publishes the message](./spec/acceptance/realtime/presence_spec.rb#L182)
        * with supported data payload content type
          * JSON Object (Hash)
            * [is encoded and decoded to the same hash](./spec/acceptance/realtime/presence_spec.rb#L209)
          * JSON Array
            * [is encoded and decoded to the same Array](./spec/acceptance/realtime/presence_spec.rb#L219)
          * String
            * [is encoded and decoded to the same Array](./spec/acceptance/realtime/presence_spec.rb#L229)
          * Binary
            * [is encoded and decoded to the same Array](./spec/acceptance/realtime/presence_spec.rb#L239)
        * with unsupported data payload content type
          * Integer
            * [raises an UnsupportedDataType 40011 exception](./spec/acceptance/realtime/presence_spec.rb#L259)
          * Float
            * [raises an UnsupportedDataType 40011 exception](./spec/acceptance/realtime/presence_spec.rb#L268)
          * Boolean
            * [raises an UnsupportedDataType 40011 exception](./spec/acceptance/realtime/presence_spec.rb#L277)
          * False
            * [raises an UnsupportedDataType 40011 exception](./spec/acceptance/realtime/presence_spec.rb#L286)
        * if connection fails before success
          * [calls the Deferrable errback if channel is detached](./spec/acceptance/realtime/presence_spec.rb#L331)
    * #leave
      * [raises an exception if not entered](./spec/acceptance/realtime/presence_spec.rb#L1201)
      * :data option
        * when set to a string
          * [emits the new data for the leave event](./spec/acceptance/realtime/presence_spec.rb#L1146)
        * when set to nil
          * [emits the last value for the data attribute when leaving](./spec/acceptance/realtime/presence_spec.rb#L1159)
        * when not passed as an argument (i.e. nil)
          * [emits the previous value for the data attribute when leaving](./spec/acceptance/realtime/presence_spec.rb#L1172)
        * and sync is complete
          * [does not cache members that have left](./spec/acceptance/realtime/presence_spec.rb#L1185)
      * it should behave like a public presence method
        * [returns a SafeDeferrable that catches exceptions in callbacks and logs them](./spec/acceptance/realtime/presence_spec.rb#L293)
        * [allows a block to be passed in that is executed upon success](./spec/acceptance/realtime/presence_spec.rb#L300)
        * [calls the Deferrable callback on success](./spec/acceptance/realtime/presence_spec.rb#L308)
        * [catches exceptions in the provided method block and logs them to the logger](./spec/acceptance/realtime/presence_spec.rb#L318)
        * with supported data payload content type
          * JSON Object (Hash)
            * [is encoded and decoded to the same hash](./spec/acceptance/realtime/presence_spec.rb#L209)
          * JSON Array
            * [is encoded and decoded to the same Array](./spec/acceptance/realtime/presence_spec.rb#L219)
          * String
            * [is encoded and decoded to the same Array](./spec/acceptance/realtime/presence_spec.rb#L229)
          * Binary
            * [is encoded and decoded to the same Array](./spec/acceptance/realtime/presence_spec.rb#L239)
        * with unsupported data payload content type
          * Integer
            * [raises an UnsupportedDataType 40011 exception](./spec/acceptance/realtime/presence_spec.rb#L259)
          * Float
            * [raises an UnsupportedDataType 40011 exception](./spec/acceptance/realtime/presence_spec.rb#L268)
          * Boolean
            * [raises an UnsupportedDataType 40011 exception](./spec/acceptance/realtime/presence_spec.rb#L277)
          * False
            * [raises an UnsupportedDataType 40011 exception](./spec/acceptance/realtime/presence_spec.rb#L286)
        * if connection fails before success
          * [calls the Deferrable errback if channel is detached](./spec/acceptance/realtime/presence_spec.rb#L331)
    * :left event
      * [emits the data defined in enter](./spec/acceptance/realtime/presence_spec.rb#L1210)
      * [emits the data defined in update](./spec/acceptance/realtime/presence_spec.rb#L1221)
    * entering/updating/leaving presence state on behalf of another client_id
      * #enter_client
        * multiple times on the same channel with different client_ids
          * [has no affect on the client's presence state and only enters on behalf of the provided client_id](./spec/acceptance/realtime/presence_spec.rb#L1244)
          * [enters a channel and sets the data based on the provided :data option](./spec/acceptance/realtime/presence_spec.rb#L1258)
        * message #connection_id
          * [matches the current client connection_id](./spec/acceptance/realtime/presence_spec.rb#L1277)
        * without necessary capabilities to enter on behalf of another client
          * [calls the Deferrable errback on capabilities failure](./spec/acceptance/realtime/presence_spec.rb#L1297)
        * it should behave like a public presence method
          * [raise an exception if the channel is detached](./spec/acceptance/realtime/presence_spec.rb#L63)
          * [raise an exception if the channel becomes detached](./spec/acceptance/realtime/presence_spec.rb#L81)
          * [raise an exception if the channel is failed](./spec/acceptance/realtime/presence_spec.rb#L97)
          * [raise an exception if the channel becomes failed](./spec/acceptance/realtime/presence_spec.rb#L114)
          * [implicitly attaches the channel](./spec/acceptance/realtime/presence_spec.rb#L130)
          * [returns a SafeDeferrable that catches exceptions in callbacks and logs them](./spec/acceptance/realtime/presence_spec.rb#L293)
          * [allows a block to be passed in that is executed upon success](./spec/acceptance/realtime/presence_spec.rb#L300)
          * [calls the Deferrable callback on success](./spec/acceptance/realtime/presence_spec.rb#L308)
          * [catches exceptions in the provided method block and logs them to the logger](./spec/acceptance/realtime/presence_spec.rb#L318)
          * when :queue_messages client option is false
            * and connection state initialized
              * [fails the deferrable](./spec/acceptance/realtime/presence_spec.rb#L142)
            * and connection state connecting
              * [fails the deferrable](./spec/acceptance/realtime/presence_spec.rb#L152)
            * and connection state disconnected
              * [fails the deferrable](./spec/acceptance/realtime/presence_spec.rb#L167)
            * and connection state connected
              * [publishes the message](./spec/acceptance/realtime/presence_spec.rb#L182)
          * with supported data payload content type
            * JSON Object (Hash)
              * [is encoded and decoded to the same hash](./spec/acceptance/realtime/presence_spec.rb#L209)
            * JSON Array
              * [is encoded and decoded to the same Array](./spec/acceptance/realtime/presence_spec.rb#L219)
            * String
              * [is encoded and decoded to the same Array](./spec/acceptance/realtime/presence_spec.rb#L229)
            * Binary
              * [is encoded and decoded to the same Array](./spec/acceptance/realtime/presence_spec.rb#L239)
          * with unsupported data payload content type
            * Integer
              * [raises an UnsupportedDataType 40011 exception](./spec/acceptance/realtime/presence_spec.rb#L259)
            * Float
              * [raises an UnsupportedDataType 40011 exception](./spec/acceptance/realtime/presence_spec.rb#L268)
            * Boolean
              * [raises an UnsupportedDataType 40011 exception](./spec/acceptance/realtime/presence_spec.rb#L277)
            * False
              * [raises an UnsupportedDataType 40011 exception](./spec/acceptance/realtime/presence_spec.rb#L286)
          * if connection fails before success
            * [calls the Deferrable errback if channel is detached](./spec/acceptance/realtime/presence_spec.rb#L331)
        * it should behave like a presence on behalf of another client method
          * :enter_client when authenticated with a wildcard client_id
            * and a valid client_id
              * [succeeds](./spec/acceptance/realtime/presence_spec.rb#L362)
            * and a wildcard client_id
              * [throws an exception](./spec/acceptance/realtime/presence_spec.rb#L372)
            * and an empty client_id
              * [throws an exception](./spec/acceptance/realtime/presence_spec.rb#L379)
            * and a client_id that is not a string type
              * [throws an exception](./spec/acceptance/realtime/presence_spec.rb#L386)
          * :enter_client when authenticated with a valid client_id
            * and another invalid client_id
              * before authentication
                * [allows the operation and then Ably rejects the operation](./spec/acceptance/realtime/presence_spec.rb#L402)
              * after authentication
                * [throws an exception](./spec/acceptance/realtime/presence_spec.rb#L411)
            * and a wildcard client_id
              * [throws an exception](./spec/acceptance/realtime/presence_spec.rb#L421)
            * and an empty client_id
              * [throws an exception](./spec/acceptance/realtime/presence_spec.rb#L428)
          * :enter_client when anonymous and no client_id
            * and another invalid client_id
              * before authentication
                * [allows the operation and then Ably rejects the operation](./spec/acceptance/realtime/presence_spec.rb#L444)
              * after authentication
                * [throws an exception](./spec/acceptance/realtime/presence_spec.rb#L453)
            * and a wildcard client_id
              * [throws an exception](./spec/acceptance/realtime/presence_spec.rb#L463)
            * and an empty client_id
              * [throws an exception](./spec/acceptance/realtime/presence_spec.rb#L470)
      * #update_client
        * multiple times on the same channel with different client_ids
          * [updates the data attribute for the member when :data option provided](./spec/acceptance/realtime/presence_spec.rb#L1311)
          * [updates the data attribute to null for the member when :data option is not provided (assumed null)](./spec/acceptance/realtime/presence_spec.rb#L1335)
          * [enters if not already entered](./spec/acceptance/realtime/presence_spec.rb#L1347)
        * it should behave like a public presence method
          * [raise an exception if the channel is detached](./spec/acceptance/realtime/presence_spec.rb#L63)
          * [raise an exception if the channel becomes detached](./spec/acceptance/realtime/presence_spec.rb#L81)
          * [raise an exception if the channel is failed](./spec/acceptance/realtime/presence_spec.rb#L97)
          * [raise an exception if the channel becomes failed](./spec/acceptance/realtime/presence_spec.rb#L114)
          * [implicitly attaches the channel](./spec/acceptance/realtime/presence_spec.rb#L130)
          * [returns a SafeDeferrable that catches exceptions in callbacks and logs them](./spec/acceptance/realtime/presence_spec.rb#L293)
          * [allows a block to be passed in that is executed upon success](./spec/acceptance/realtime/presence_spec.rb#L300)
          * [calls the Deferrable callback on success](./spec/acceptance/realtime/presence_spec.rb#L308)
          * [catches exceptions in the provided method block and logs them to the logger](./spec/acceptance/realtime/presence_spec.rb#L318)
          * when :queue_messages client option is false
            * and connection state initialized
              * [fails the deferrable](./spec/acceptance/realtime/presence_spec.rb#L142)
            * and connection state connecting
              * [fails the deferrable](./spec/acceptance/realtime/presence_spec.rb#L152)
            * and connection state disconnected
              * [fails the deferrable](./spec/acceptance/realtime/presence_spec.rb#L167)
            * and connection state connected
              * [publishes the message](./spec/acceptance/realtime/presence_spec.rb#L182)
          * with supported data payload content type
            * JSON Object (Hash)
              * [is encoded and decoded to the same hash](./spec/acceptance/realtime/presence_spec.rb#L209)
            * JSON Array
              * [is encoded and decoded to the same Array](./spec/acceptance/realtime/presence_spec.rb#L219)
            * String
              * [is encoded and decoded to the same Array](./spec/acceptance/realtime/presence_spec.rb#L229)
            * Binary
              * [is encoded and decoded to the same Array](./spec/acceptance/realtime/presence_spec.rb#L239)
          * with unsupported data payload content type
            * Integer
              * [raises an UnsupportedDataType 40011 exception](./spec/acceptance/realtime/presence_spec.rb#L259)
            * Float
              * [raises an UnsupportedDataType 40011 exception](./spec/acceptance/realtime/presence_spec.rb#L268)
            * Boolean
              * [raises an UnsupportedDataType 40011 exception](./spec/acceptance/realtime/presence_spec.rb#L277)
            * False
              * [raises an UnsupportedDataType 40011 exception](./spec/acceptance/realtime/presence_spec.rb#L286)
          * if connection fails before success
            * [calls the Deferrable errback if channel is detached](./spec/acceptance/realtime/presence_spec.rb#L331)
        * it should behave like a presence on behalf of another client method
          * :update_client when authenticated with a wildcard client_id
            * and a valid client_id
              * [succeeds](./spec/acceptance/realtime/presence_spec.rb#L362)
            * and a wildcard client_id
              * [throws an exception](./spec/acceptance/realtime/presence_spec.rb#L372)
            * and an empty client_id
              * [throws an exception](./spec/acceptance/realtime/presence_spec.rb#L379)
            * and a client_id that is not a string type
              * [throws an exception](./spec/acceptance/realtime/presence_spec.rb#L386)
          * :update_client when authenticated with a valid client_id
            * and another invalid client_id
              * before authentication
                * [allows the operation and then Ably rejects the operation](./spec/acceptance/realtime/presence_spec.rb#L402)
              * after authentication
                * [throws an exception](./spec/acceptance/realtime/presence_spec.rb#L411)
            * and a wildcard client_id
              * [throws an exception](./spec/acceptance/realtime/presence_spec.rb#L421)
            * and an empty client_id
              * [throws an exception](./spec/acceptance/realtime/presence_spec.rb#L428)
          * :update_client when anonymous and no client_id
            * and another invalid client_id
              * before authentication
                * [allows the operation and then Ably rejects the operation](./spec/acceptance/realtime/presence_spec.rb#L444)
              * after authentication
                * [throws an exception](./spec/acceptance/realtime/presence_spec.rb#L453)
            * and a wildcard client_id
              * [throws an exception](./spec/acceptance/realtime/presence_spec.rb#L463)
            * and an empty client_id
              * [throws an exception](./spec/acceptance/realtime/presence_spec.rb#L470)
      * #leave_client
        * leaves a channel
          * multiple times on the same channel with different client_ids
            * [emits the :leave event for each client_id](./spec/acceptance/realtime/presence_spec.rb#L1377)
            * [succeeds if that client_id has not previously entered the channel](./spec/acceptance/realtime/presence_spec.rb#L1401)
          * with a new value in :data option
            * [emits the leave event with the new data value](./spec/acceptance/realtime/presence_spec.rb#L1425)
          * with a nil value in :data option
            * [emits the leave event with the previous value as a convenience](./spec/acceptance/realtime/presence_spec.rb#L1438)
          * with no :data option
            * [emits the leave event with the previous value as a convenience](./spec/acceptance/realtime/presence_spec.rb#L1451)
        * it should behave like a public presence method
          * [raise an exception if the channel is detached](./spec/acceptance/realtime/presence_spec.rb#L63)
          * [raise an exception if the channel becomes detached](./spec/acceptance/realtime/presence_spec.rb#L81)
          * [raise an exception if the channel is failed](./spec/acceptance/realtime/presence_spec.rb#L97)
          * [raise an exception if the channel becomes failed](./spec/acceptance/realtime/presence_spec.rb#L114)
          * [implicitly attaches the channel](./spec/acceptance/realtime/presence_spec.rb#L130)
          * [returns a SafeDeferrable that catches exceptions in callbacks and logs them](./spec/acceptance/realtime/presence_spec.rb#L293)
          * [allows a block to be passed in that is executed upon success](./spec/acceptance/realtime/presence_spec.rb#L300)
          * [calls the Deferrable callback on success](./spec/acceptance/realtime/presence_spec.rb#L308)
          * [catches exceptions in the provided method block and logs them to the logger](./spec/acceptance/realtime/presence_spec.rb#L318)
          * when :queue_messages client option is false
            * and connection state initialized
              * [fails the deferrable](./spec/acceptance/realtime/presence_spec.rb#L142)
            * and connection state connecting
              * [fails the deferrable](./spec/acceptance/realtime/presence_spec.rb#L152)
            * and connection state disconnected
              * [fails the deferrable](./spec/acceptance/realtime/presence_spec.rb#L167)
            * and connection state connected
              * [publishes the message](./spec/acceptance/realtime/presence_spec.rb#L182)
          * with supported data payload content type
            * JSON Object (Hash)
              * [is encoded and decoded to the same hash](./spec/acceptance/realtime/presence_spec.rb#L209)
            * JSON Array
              * [is encoded and decoded to the same Array](./spec/acceptance/realtime/presence_spec.rb#L219)
            * String
              * [is encoded and decoded to the same Array](./spec/acceptance/realtime/presence_spec.rb#L229)
            * Binary
              * [is encoded and decoded to the same Array](./spec/acceptance/realtime/presence_spec.rb#L239)
          * with unsupported data payload content type
            * Integer
              * [raises an UnsupportedDataType 40011 exception](./spec/acceptance/realtime/presence_spec.rb#L259)
            * Float
              * [raises an UnsupportedDataType 40011 exception](./spec/acceptance/realtime/presence_spec.rb#L268)
            * Boolean
              * [raises an UnsupportedDataType 40011 exception](./spec/acceptance/realtime/presence_spec.rb#L277)
            * False
              * [raises an UnsupportedDataType 40011 exception](./spec/acceptance/realtime/presence_spec.rb#L286)
          * if connection fails before success
            * [calls the Deferrable errback if channel is detached](./spec/acceptance/realtime/presence_spec.rb#L331)
        * it should behave like a presence on behalf of another client method
          * :leave_client when authenticated with a wildcard client_id
            * and a valid client_id
              * [succeeds](./spec/acceptance/realtime/presence_spec.rb#L362)
            * and a wildcard client_id
              * [throws an exception](./spec/acceptance/realtime/presence_spec.rb#L372)
            * and an empty client_id
              * [throws an exception](./spec/acceptance/realtime/presence_spec.rb#L379)
            * and a client_id that is not a string type
              * [throws an exception](./spec/acceptance/realtime/presence_spec.rb#L386)
          * :leave_client when authenticated with a valid client_id
            * and another invalid client_id
              * before authentication
                * [allows the operation and then Ably rejects the operation](./spec/acceptance/realtime/presence_spec.rb#L402)
              * after authentication
                * [throws an exception](./spec/acceptance/realtime/presence_spec.rb#L411)
            * and a wildcard client_id
              * [throws an exception](./spec/acceptance/realtime/presence_spec.rb#L421)
            * and an empty client_id
              * [throws an exception](./spec/acceptance/realtime/presence_spec.rb#L428)
          * :leave_client when anonymous and no client_id
            * and another invalid client_id
              * before authentication
                * [allows the operation and then Ably rejects the operation](./spec/acceptance/realtime/presence_spec.rb#L444)
              * after authentication
                * [throws an exception](./spec/acceptance/realtime/presence_spec.rb#L453)
            * and a wildcard client_id
              * [throws an exception](./spec/acceptance/realtime/presence_spec.rb#L463)
            * and an empty client_id
              * [throws an exception](./spec/acceptance/realtime/presence_spec.rb#L470)
    * #get
      * [returns a SafeDeferrable that catches exceptions in callbacks and logs them](./spec/acceptance/realtime/presence_spec.rb#L1470)
      * [calls the Deferrable callback on success](./spec/acceptance/realtime/presence_spec.rb#L1475)
      * [catches exceptions in the provided method block](./spec/acceptance/realtime/presence_spec.rb#L1482)
      * [implicitly attaches the channel (#RTP11b)](./spec/acceptance/realtime/presence_spec.rb#L1490)
      * [fails if the connection is DETACHED (#RTP11b)](./spec/acceptance/realtime/presence_spec.rb#L1524)
      * [fails if the connection is FAILED (#RTP11b)](./spec/acceptance/realtime/presence_spec.rb#L1539)
      * [returns the current members on the channel (#RTP11a)](./spec/acceptance/realtime/presence_spec.rb#L1623)
      * [filters by connection_id option if provided (#RTP11c3)](./spec/acceptance/realtime/presence_spec.rb#L1638)
      * [filters by client_id option if provided (#RTP11c2)](./spec/acceptance/realtime/presence_spec.rb#L1660)
      * [does not wait for SYNC to complete if :wait_for_sync option is false (#RTP11c1)](./spec/acceptance/realtime/presence_spec.rb#L1684)
      * [returns the list of members and waits for SYNC to complete by default (#RTP11a)](./spec/acceptance/realtime/presence_spec.rb#L1693)
      * when the channel is SUSPENDED
        * with wait_for_sync: true
          * [returns the current PresenceMap and does not wait for the channel to change to the ATTACHED state (#RTP11d)](./spec/acceptance/realtime/presence_spec.rb#L1500)
        * with wait_for_sync: false
          * [returns the current PresenceMap and does not wait for the channel to change to the ATTACHED state (#RTP11d)](./spec/acceptance/realtime/presence_spec.rb#L1512)
      * during a sync
        * when :wait_for_sync is true
          * [fails if the connection becomes FAILED (#RTP11b)](./spec/acceptance/realtime/presence_spec.rb#L1576)
          * [fails if the channel becomes detached (#RTP11b)](./spec/acceptance/realtime/presence_spec.rb#L1599)
      * when a member enters and then leaves
        * [has no members](./spec/acceptance/realtime/presence_spec.rb#L1703)
      * when a member enters and the presence map is updated
        * [adds the member as being :present (#RTP2d)](./spec/acceptance/realtime/presence_spec.rb#L1716)
      * with lots of members on different clients
        * [returns a complete list of members on all clients](./spec/acceptance/realtime/presence_spec.rb#L1734)
    * #subscribe
      * [implicitly attaches](./spec/acceptance/realtime/presence_spec.rb#L1809)
      * with no arguments
        * [calls the callback for all presence events](./spec/acceptance/realtime/presence_spec.rb#L1770)
      * with event name
        * [calls the callback for specified presence event](./spec/acceptance/realtime/presence_spec.rb#L1790)
      * with a callback that raises an exception
        * [logs the error and continues](./spec/acceptance/realtime/presence_spec.rb#L1822)
    * #unsubscribe
      * with no arguments
        * [removes the callback for all presence events](./spec/acceptance/realtime/presence_spec.rb#L1843)
      * with event name
        * [removes the callback for specified presence event](./spec/acceptance/realtime/presence_spec.rb#L1861)
    * REST #get
      * [returns current members](./spec/acceptance/realtime/presence_spec.rb#L1880)
      * [returns no members once left](./spec/acceptance/realtime/presence_spec.rb#L1893)
    * client_id with ASCII_8BIT
      * in connection set up
        * [is converted into UTF_8](./spec/acceptance/realtime/presence_spec.rb#L1910)
      * in channel options
        * [is converted into UTF_8](./spec/acceptance/realtime/presence_spec.rb#L1923)
    * encoding and decoding of presence message data
      * [encrypts presence message data](./spec/acceptance/realtime/presence_spec.rb#L1949)
      * #subscribe
        * [emits decrypted enter events](./spec/acceptance/realtime/presence_spec.rb#L1968)
        * [emits decrypted update events](./spec/acceptance/realtime/presence_spec.rb#L1980)
        * [emits previously set data for leave events](./spec/acceptance/realtime/presence_spec.rb#L1994)
      * #get
        * [returns a list of members with decrypted data](./spec/acceptance/realtime/presence_spec.rb#L2010)
      * REST #get
        * [returns a list of members with decrypted data](./spec/acceptance/realtime/presence_spec.rb#L2023)
      * when cipher settings do not match publisher
        * [delivers an unencoded presence message left with encoding value](./spec/acceptance/realtime/presence_spec.rb#L2038)
        * [emits an error when cipher does not match and presence data cannot be decoded](./spec/acceptance/realtime/presence_spec.rb#L2051)
    * leaving
      * [expect :left event once underlying connection is closed](./spec/acceptance/realtime/presence_spec.rb#L2067)
      * [expect :left event with client data from enter event](./spec/acceptance/realtime/presence_spec.rb#L2077)
    * connection failure mid-way through a large member sync
      * [resumes the SYNC operation (#RTP3)](./spec/acceptance/realtime/presence_spec.rb#L2096)
    * server-initiated sync
      * with multiple SYNC pages
        * [is initiated with a SYNC message and completed with a later SYNC message with no cursor value part of the channelSerial (#RTP18a, #RTP18b) ](./spec/acceptance/realtime/presence_spec.rb#L2134)
      * with a single SYNC page
        * [is initiated and completed with a single SYNC message (and no channelSerial) (#RTP18a, #RTP18c) ](./spec/acceptance/realtime/presence_spec.rb#L2185)
      * when members exist in the PresenceMap before a SYNC completes
        * [removes the members that are no longer present (#RTP19)](./spec/acceptance/realtime/presence_spec.rb#L2233)
    * when the client does not have presence subscribe privileges but is present on the channel
      * PENDING: *[receives presence updates for all presence events generated by the current connection and the presence map is kept up to date (#RTP17a)](./spec/acceptance/realtime/presence_spec.rb#L2291)*
    * local PresenceMap for presence members entered by this client
      * [maintains a copy of the member map for any member that shares this connection's connection ID (#RTP17)](./spec/acceptance/realtime/presence_spec.rb#L2328)
      * when a channel becomes attached again
        * and the resume flag is true
          * and the presence flag is false
            * [does not send any presence events as the PresenceMap is in sync (#RTP5c1)](./spec/acceptance/realtime/presence_spec.rb#L2368)
          * and the presence flag is true
            * and following the SYNC all local MemberMap members are present in the PresenceMap
              * [does nothing as MemberMap is in sync (#RTP5c2)](./spec/acceptance/realtime/presence_spec.rb#L2395)
            * and following the SYNC a local MemberMap member is not present in the PresenceMap
              * [re-enters the missing members automatically (#RTP5c2)](./spec/acceptance/realtime/presence_spec.rb#L2431)
        * and the resume flag is false
          * and the presence flag is false
            * [immediately resends all local presence members (#RTP5c2, #RTP19a)](./spec/acceptance/realtime/presence_spec.rb#L2511)
        * when re-entering a client automatically, if the re-enter fails for any reason
          * [should emit an ErrorInfo with error code 91004 (#RTP5c3)](./spec/acceptance/realtime/presence_spec.rb#L2559)
    * channel state side effects
      * channel transitions to the FAILED state
        * [clears the PresenceMap and local member map copy and does not emit any presence events (#RTP5a)](./spec/acceptance/realtime/presence_spec.rb#L2616)
      * channel transitions to the DETACHED state
        * [clears the PresenceMap and local member map copy and does not emit any presence events (#RTP5a)](./spec/acceptance/realtime/presence_spec.rb#L2640)
      * channel transitions to the SUSPENDED state
        * [maintains the PresenceMap and only publishes presence event changes since the last attached state (#RTP5f)](./spec/acceptance/realtime/presence_spec.rb#L2673)

### Ably::Realtime::Client#stats
_(see [spec/acceptance/realtime/stats_spec.rb](./spec/acceptance/realtime/stats_spec.rb))_
  * using JSON protocol
    * fetching stats
      * [returns a PaginatedResult](./spec/acceptance/realtime/stats_spec.rb#L10)
      * [returns a SafeDeferrable that catches exceptions in callbacks and logs them](./spec/acceptance/realtime/stats_spec.rb#L29)
      * with options
        * [passes the option arguments to the REST stat method](./spec/acceptance/realtime/stats_spec.rb#L20)

### Ably::Realtime::Client#time
_(see [spec/acceptance/realtime/time_spec.rb](./spec/acceptance/realtime/time_spec.rb))_
  * using JSON protocol
    * fetching the service time
      * [should return the service time as a Time object](./spec/acceptance/realtime/time_spec.rb#L10)
      * [returns a SafeDeferrable that catches exceptions in callbacks and logs them](./spec/acceptance/realtime/time_spec.rb#L19)
      * with reconfigured HTTP timeout
        * [should raise a timeout exception](./spec/acceptance/realtime/time_spec.rb#L31)

### Ably::Auth
_(see [spec/acceptance/rest/auth_spec.rb](./spec/acceptance/rest/auth_spec.rb))_
  * using JSON protocol
    * [has immutable options](./spec/acceptance/rest/auth_spec.rb#L60)
    * #request_token
      * [creates a TokenRequest automatically and sends it to Ably to obtain a token](./spec/acceptance/rest/auth_spec.rb#L75)
      * [returns a valid TokenDetails object in the expected format with valid issued and expires attributes](./spec/acceptance/rest/auth_spec.rb#L84)
      * with token_param :client_id
        * [overrides default and uses camelCase notation for attributes](./spec/acceptance/rest/auth_spec.rb#L117)
      * with token_param :capability
        * [overrides default and uses camelCase notation for attributes](./spec/acceptance/rest/auth_spec.rb#L117)
      * with token_param :nonce
        * [overrides default and uses camelCase notation for attributes](./spec/acceptance/rest/auth_spec.rb#L117)
      * with token_param :timestamp
        * [overrides default and uses camelCase notation for attributes](./spec/acceptance/rest/auth_spec.rb#L117)
      * with token_param :ttl
        * [overrides default and uses camelCase notation for attributes](./spec/acceptance/rest/auth_spec.rb#L117)
      * with :key option
        * [key_name is used in request and signing uses key_secret](./spec/acceptance/rest/auth_spec.rb#L147)
      * with :key_name & :key_secret options
        * [key_name is used in request and signing uses key_secret](./spec/acceptance/rest/auth_spec.rb#L177)
      * with :query_time option
        * [queries the server for the time (#RSA10k)](./spec/acceptance/rest/auth_spec.rb#L185)
      * without :query_time option
        * [does not query the server for the time](./spec/acceptance/rest/auth_spec.rb#L194)
      * with :auth_url option merging
        * with existing configured auth options
          * using unspecified :auth_method
            * [requests a token using a GET request with provided headers, and merges client_id into auth_params](./spec/acceptance/rest/auth_spec.rb#L234)
            * with provided token_params
              * [merges provided token_params with existing auth_params and client_id](./spec/acceptance/rest/auth_spec.rb#L242)
            * with provided auth option auth_params and auth_headers
              * [replaces any preconfigured auth_params](./spec/acceptance/rest/auth_spec.rb#L250)
          * using :get :auth_method and query params in the URL
            * [requests a token using a GET request with provided headers, and merges client_id into auth_params and existing URL querystring into new URL querystring](./spec/acceptance/rest/auth_spec.rb#L261)
          * using :post :auth_method
            * [requests a token using a POST request with provided headers, and merges client_id into auth_params as form-encoded post data](./spec/acceptance/rest/auth_spec.rb#L271)
      * with :auth_url option
        * when response from :auth_url is a valid token request
          * [requests a token from :auth_url using an HTTP GET request](./spec/acceptance/rest/auth_spec.rb#L321)
          * [returns a valid token generated from the token request](./spec/acceptance/rest/auth_spec.rb#L326)
          * with :query_params
            * [requests a token from :auth_url with the :query_params](./spec/acceptance/rest/auth_spec.rb#L333)
          * with :headers
            * [requests a token from :auth_url with the HTTP headers set](./spec/acceptance/rest/auth_spec.rb#L341)
          * with POST
            * [requests a token from :auth_url using an HTTP POST instead of the default GET](./spec/acceptance/rest/auth_spec.rb#L349)
        * when response from :auth_url is a token details object
          * [returns TokenDetails created from the token JSON](./spec/acceptance/rest/auth_spec.rb#L374)
        * when response from :auth_url is text/plain content type and a token string
          * [returns TokenDetails created from the token JSON](./spec/acceptance/rest/auth_spec.rb#L392)
        * when response is invalid
          * 500
            * [raises ServerError](./spec/acceptance/rest/auth_spec.rb#L406)
          * XML
            * [raises InvalidResponseBody](./spec/acceptance/rest/auth_spec.rb#L417)
      * with a Proc for the :auth_callback option
        * that returns a TokenRequest
          * [calls the Proc with token_params when authenticating to obtain the request token](./spec/acceptance/rest/auth_spec.rb#L440)
          * [uses the token request returned from the callback when requesting a new token](./spec/acceptance/rest/auth_spec.rb#L444)
          * when authorized
            * [sets Auth#client_id to the new token's client_id](./spec/acceptance/rest/auth_spec.rb#L451)
            * [sets Client#client_id to the new token's client_id](./spec/acceptance/rest/auth_spec.rb#L455)
        * that returns a TokenDetails JSON object
          * [calls the Proc when authenticating to obtain the request token](./spec/acceptance/rest/auth_spec.rb#L489)
          * [uses the token request returned from the callback when requesting a new token](./spec/acceptance/rest/auth_spec.rb#L494)
          * when authorized
            * [sets Auth#client_id to the new token's client_id](./spec/acceptance/rest/auth_spec.rb#L506)
            * [sets Client#client_id to the new token's client_id](./spec/acceptance/rest/auth_spec.rb#L510)
        * that returns a TokenDetails object
          * [uses the token request returned from the callback when requesting a new token](./spec/acceptance/rest/auth_spec.rb#L525)
        * that returns a Token string
          * [uses the token request returned from the callback when requesting a new token](./spec/acceptance/rest/auth_spec.rb#L541)
      * with auth_option :client_id
        * [returns a token with the client_id](./spec/acceptance/rest/auth_spec.rb#L571)
      * with token_param :client_id
        * [returns a token with the client_id](./spec/acceptance/rest/auth_spec.rb#L580)
    * before #authorize has been called
      * [has no current_token_details](./spec/acceptance/rest/auth_spec.rb#L587)
    * #authorize (#RSA10, #RSA10j)
      * [updates the persisted token params that are then used for subsequent authorize requests](./spec/acceptance/rest/auth_spec.rb#L754)
      * [updates the persisted auth options that are then used for subsequent authorize requests](./spec/acceptance/rest/auth_spec.rb#L760)
      * when called for the first time since the client has been instantiated
        * [passes all auth_options and token_params to #request_token](./spec/acceptance/rest/auth_spec.rb#L601)
        * [returns a valid token](./spec/acceptance/rest/auth_spec.rb#L606)
        * [issues a new token every time (#RSA10a)](./spec/acceptance/rest/auth_spec.rb#L610)
      * query_time: true with authorize
        * [only queries the server time once and then works out the offset, query_time option is never persisted (#RSA10k)](./spec/acceptance/rest/auth_spec.rb#L624)
      * query_time: true ClientOption when instanced
        * [only queries the server time once and then works out the offset, query_time option is never persisted (#RSA10k)](./spec/acceptance/rest/auth_spec.rb#L644)
      * TokenParams argument
        * [has no effect on the defaults when null and TokenParam defaults remain the same](./spec/acceptance/rest/auth_spec.rb#L661)
        * [updates defaults when present and all previous configured TokenParams are discarded (#RSA10g)](./spec/acceptance/rest/auth_spec.rb#L668)
        * [updates Auth#token_params attribute with an immutable hash](./spec/acceptance/rest/auth_spec.rb#L676)
        * [uses TokenParams#timestamp for this request but obtains a new timestamp for subsequence requests (#RSA10g)](./spec/acceptance/rest/auth_spec.rb#L681)
      * AuthOptions argument
        * [has no effect on the defaults when null and AuthOptions defaults remain the same](./spec/acceptance/rest/auth_spec.rb#L706)
        * [updates defaults when present and all previous configured AuthOptions are discarded (#RSA10g)](./spec/acceptance/rest/auth_spec.rb#L712)
        * [updates Auth#options attribute with an immutable hash](./spec/acceptance/rest/auth_spec.rb#L719)
        * [uses AuthOptions#query_time for this request and will not query_time for subsequent requests (#RSA10g)](./spec/acceptance/rest/auth_spec.rb#L724)
        * [uses AuthOptions#query_time for this request and will query_time again if provided subsequently](./spec/acceptance/rest/auth_spec.rb#L730)
      * with previous authorisation
        * [requests a new token if token is expired](./spec/acceptance/rest/auth_spec.rb#L743)
        * [issues a new token every time #authorize is called](./spec/acceptance/rest/auth_spec.rb#L749)
      * with a Proc for the :auth_callback option
        * [calls the Proc](./spec/acceptance/rest/auth_spec.rb#L776)
        * [uses the token request returned from the callback when requesting a new token](./spec/acceptance/rest/auth_spec.rb#L780)
        * for every subsequent #request_token
          * without a :auth_callback Proc
            * [calls the originally provided block](./spec/acceptance/rest/auth_spec.rb#L786)
          * with a provided block
            * [does not call the originally provided Proc and calls the new #request_token :auth_callback Proc](./spec/acceptance/rest/auth_spec.rb#L793)
      * with an explicit token string that expires
        * and a Proc for the :auth_callback option to provide a means to renew the token
          * [calls the Proc once the token has expired and the new token is used](./spec/acceptance/rest/auth_spec.rb#L820)
      * with an explicit ClientOptions client_id
        * and an incompatible client_id in a TokenDetails object passed to the auth callback
          * [rejects a TokenDetails object with an incompatible client_id and raises an exception](./spec/acceptance/rest/auth_spec.rb#L838)
        * and an incompatible client_id in a TokenRequest object passed to the auth callback and raises an exception
          * [rejects a TokenRequests object with an incompatible client_id and raises an exception](./spec/acceptance/rest/auth_spec.rb#L846)
        * and a token string without any retrievable client_id
          * [rejects a TokenRequests object with an incompatible client_id and raises an exception](./spec/acceptance/rest/auth_spec.rb#L854)
    * #create_token_request
      * [returns a TokenRequest object](./spec/acceptance/rest/auth_spec.rb#L869)
      * [returns a TokenRequest that can be passed to a client that can use it for authentication without an API key](./spec/acceptance/rest/auth_spec.rb#L873)
      * [uses the key name from the client](./spec/acceptance/rest/auth_spec.rb#L880)
      * [uses the default TTL](./spec/acceptance/rest/auth_spec.rb#L884)
      * [uses the default capability](./spec/acceptance/rest/auth_spec.rb#L897)
      * with a :ttl option below the Token expiry buffer that ensures tokens are renewed 15s before they expire as they are considered expired
        * [uses the Token expiry buffer default + 10s to allow for a token request in flight](./spec/acceptance/rest/auth_spec.rb#L891)
      * the nonce
        * [is unique for every request](./spec/acceptance/rest/auth_spec.rb#L902)
        * [is at least 16 characters](./spec/acceptance/rest/auth_spec.rb#L907)
      * with token param :ttl
        * [overrides default](./spec/acceptance/rest/auth_spec.rb#L918)
      * with token param :nonce
        * [overrides default](./spec/acceptance/rest/auth_spec.rb#L918)
      * with token param :client_id
        * [overrides default](./spec/acceptance/rest/auth_spec.rb#L918)
      * when specifying capability
        * [overrides the default](./spec/acceptance/rest/auth_spec.rb#L929)
        * [uses these capabilities when Ably issues an actual token](./spec/acceptance/rest/auth_spec.rb#L933)
      * with additional invalid attributes
        * [are ignored](./spec/acceptance/rest/auth_spec.rb#L943)
      * when required fields are missing
        * [should raise an exception if key secret is missing](./spec/acceptance/rest/auth_spec.rb#L954)
        * [should raise an exception if key name is missing](./spec/acceptance/rest/auth_spec.rb#L958)
      * timestamp attribute
        * [is a Time object in Ruby and is set to the local time](./spec/acceptance/rest/auth_spec.rb#L985)
        * with :query_time auth_option
          * [queries the server for the timestamp](./spec/acceptance/rest/auth_spec.rb#L970)
        * with :timestamp option
          * [uses the provided timestamp in the token request](./spec/acceptance/rest/auth_spec.rb#L980)
      * signing
        * [generates a valid HMAC](./spec/acceptance/rest/auth_spec.rb#L1009)
        * lexicographic ordering of channels and operations
          * [HMAC is lexicographic ordered and thus the HMAC is identical](./spec/acceptance/rest/auth_spec.rb#L1036)
          * [is valid when used for authentication](./spec/acceptance/rest/auth_spec.rb#L1042)
    * using token authentication
      * with :token option
        * [authenticates successfully using the provided :token](./spec/acceptance/rest/auth_spec.rb#L1069)
        * [disallows publishing on unspecified capability channels](./spec/acceptance/rest/auth_spec.rb#L1073)
        * [fails if timestamp is invalid](./spec/acceptance/rest/auth_spec.rb#L1081)
        * [cannot be renewed automatically](./spec/acceptance/rest/auth_spec.rb#L1089)
        * and the token expires
          * [should indicate an error and not retry the request (#RSA4a)](./spec/acceptance/rest/auth_spec.rb#L1123)
      * when implicit as a result of using :client_id
        * and requests to the Ably server are mocked
          * [will send a token request to the server](./spec/acceptance/rest/auth_spec.rb#L1153)
        * a token is created
          * [before a request is made](./spec/acceptance/rest/auth_spec.rb#L1162)
          * [when a message is published](./spec/acceptance/rest/auth_spec.rb#L1166)
          * [with capability and TTL defaults](./spec/acceptance/rest/auth_spec.rb#L1170)
          * [#client_id contains the client_id](./spec/acceptance/rest/auth_spec.rb#L1181)
      * when token expires
        * [automatically renews the token (#RSA4b)](./spec/acceptance/rest/auth_spec.rb#L1210)
        * [fails if the token renewal fails (#RSA4b)](./spec/acceptance/rest/auth_spec.rb#L1220)
      * when :client_id is provided in a token
        * [#client_id contains the client_id](./spec/acceptance/rest/auth_spec.rb#L1244)
    * #client_id_validated?
      * when using basic auth
        * [is false as basic auth users do not have an identity](./spec/acceptance/rest/auth_spec.rb#L1256)
      * when using a token auth string for a token with a client_id
        * [is false as identification is not possible from an opaque token string](./spec/acceptance/rest/auth_spec.rb#L1264)
      * when using a token
        * with a client_id
          * [is true](./spec/acceptance/rest/auth_spec.rb#L1273)
        * with no client_id (anonymous)
          * [is true](./spec/acceptance/rest/auth_spec.rb#L1281)
        * with a wildcard client_id (anonymous)
          * [is false](./spec/acceptance/rest/auth_spec.rb#L1289)
      * when using a token request with a client_id
        * [is not true as identification is not confirmed until authenticated](./spec/acceptance/rest/auth_spec.rb#L1298)
        * after authentication
          * [is true as identification is completed during implicit authentication](./spec/acceptance/rest/auth_spec.rb#L1305)
    * when using a :key and basic auth
      * [#using_token_auth? is false](./spec/acceptance/rest/auth_spec.rb#L1313)
      * [#key attribute contains the key string](./spec/acceptance/rest/auth_spec.rb#L1317)
      * [#using_basic_auth? is true](./spec/acceptance/rest/auth_spec.rb#L1321)
    * deprecated #authorise
      * [logs a deprecation warning (#RSA10l)](./spec/acceptance/rest/auth_spec.rb#L1357)
      * [returns a valid token (#RSA10l)](./spec/acceptance/rest/auth_spec.rb#L1362)

### Ably::Rest
_(see [spec/acceptance/rest/base_spec.rb](./spec/acceptance/rest/base_spec.rb))_
  * transport protocol
    * when protocol is not defined it defaults to :msgpack
      * [uses MsgPack](./spec/acceptance/rest/base_spec.rb#L27)
    * when option {:protocol=>:json} is used
      * [uses JSON](./spec/acceptance/rest/base_spec.rb#L43)
    * when option {:use_binary_protocol=>false} is used
      * [uses JSON](./spec/acceptance/rest/base_spec.rb#L43)
    * when option {:protocol=>:msgpack} is used
      * [uses MsgPack](./spec/acceptance/rest/base_spec.rb#L60)
    * when option {:use_binary_protocol=>true} is used
      * [uses MsgPack](./spec/acceptance/rest/base_spec.rb#L60)
  * using JSON protocol
    * failed requests
      * due to invalid Auth
        * [should raise an InvalidRequest exception with a valid error message and code](./spec/acceptance/rest/base_spec.rb#L75)
      * server error with JSON error response body
        * [should raise a ServerError exception](./spec/acceptance/rest/base_spec.rb#L94)
      * 500 server error without a valid JSON response body
        * [should raise a ServerError exception](./spec/acceptance/rest/base_spec.rb#L105)
    * token authentication failures
      * when auth#token_renewable?
        * [should automatically reissue a token](./spec/acceptance/rest/base_spec.rb#L143)
      * when NOT auth#token_renewable?
        * [should raise an TokenExpired exception](./spec/acceptance/rest/base_spec.rb#L158)

### Ably::Rest::Channel
_(see [spec/acceptance/rest/channel_spec.rb](./spec/acceptance/rest/channel_spec.rb))_
  * using JSON protocol
    * #publish
      * with name and data arguments
        * [publishes the message and return true indicating success](./spec/acceptance/rest/channel_spec.rb#L21)
        * and additional attributes
          * [publishes the message with the attributes and return true indicating success](./spec/acceptance/rest/channel_spec.rb#L30)
      * with a client_id configured in the ClientOptions
        * [publishes the message without a client_id](./spec/acceptance/rest/channel_spec.rb#L41)
        * [expects a client_id to be added by the realtime service](./spec/acceptance/rest/channel_spec.rb#L49)
      * with an array of Hash objects with :name and :data attributes
        * [publishes an array of messages in one HTTP request](./spec/acceptance/rest/channel_spec.rb#L62)
      * with an array of Message objects
        * [publishes an array of messages in one HTTP request](./spec/acceptance/rest/channel_spec.rb#L77)
      * without adequate permissions on the channel
        * [raises a permission error when publishing](./spec/acceptance/rest/channel_spec.rb#L89)
      * null attributes
        * when name is null
          * [publishes the message without a name attribute in the payload](./spec/acceptance/rest/channel_spec.rb#L98)
        * when data is null
          * [publishes the message without a data attribute in the payload](./spec/acceptance/rest/channel_spec.rb#L109)
        * with neither name or data attributes
          * [publishes the message without any attributes in the payload](./spec/acceptance/rest/channel_spec.rb#L120)
      * identified clients
        * when authenticated with a wildcard client_id
          * with a valid client_id in the message
            * [succeeds](./spec/acceptance/rest/channel_spec.rb#L137)
          * with a wildcard client_id in the message
            * [throws an exception](./spec/acceptance/rest/channel_spec.rb#L146)
          * with an empty client_id in the message
            * [succeeds and publishes without a client_id](./spec/acceptance/rest/channel_spec.rb#L152)
        * when authenticated with a Token string with an implicit client_id
          * without having a confirmed identity
            * with a valid client_id in the message
              * [succeeds](./spec/acceptance/rest/channel_spec.rb#L169)
            * with an invalid client_id in the message
              * [succeeds in the client library but then fails when published to Ably](./spec/acceptance/rest/channel_spec.rb#L178)
            * with an empty client_id in the message
              * [succeeds and publishes with an implicit client_id](./spec/acceptance/rest/channel_spec.rb#L184)
        * when authenticated with TokenDetails with a valid client_id
          * with a valid client_id in the message
            * [succeeds](./spec/acceptance/rest/channel_spec.rb#L201)
          * with a wildcard client_id in the message
            * [throws an exception](./spec/acceptance/rest/channel_spec.rb#L210)
          * with an invalid client_id in the message
            * [throws an exception](./spec/acceptance/rest/channel_spec.rb#L216)
          * with an empty client_id in the message
            * [succeeds and publishes with an implicit client_id](./spec/acceptance/rest/channel_spec.rb#L222)
        * when anonymous and no client_id
          * with a client_id in the message
            * [throws an exception](./spec/acceptance/rest/channel_spec.rb#L238)
          * with a wildcard client_id in the message
            * [throws an exception](./spec/acceptance/rest/channel_spec.rb#L244)
          * with an empty client_id in the message
            * [succeeds and publishes with an implicit client_id](./spec/acceptance/rest/channel_spec.rb#L250)
    * #history
      * [returns a PaginatedResult model](./spec/acceptance/rest/channel_spec.rb#L278)
      * [returns the current message history for the channel](./spec/acceptance/rest/channel_spec.rb#L282)
      * [returns paged history using the PaginatedResult model](./spec/acceptance/rest/channel_spec.rb#L310)
      * message timestamps
        * [are after the messages were published](./spec/acceptance/rest/channel_spec.rb#L295)
      * message IDs
        * [is unique](./spec/acceptance/rest/channel_spec.rb#L303)
      * direction
        * [returns paged history backwards by default](./spec/acceptance/rest/channel_spec.rb#L331)
        * [returns history forward if specified in the options](./spec/acceptance/rest/channel_spec.rb#L337)
      * limit
        * [defaults to 100](./spec/acceptance/rest/channel_spec.rb#L349)
    * #history option
      * :start
        * with milliseconds since epoch value
          * [uses this value in the history request](./spec/acceptance/rest/channel_spec.rb#L389)
        * with a Time object value
          * [converts the value to milliseconds since epoch in the hisotry request](./spec/acceptance/rest/channel_spec.rb#L399)
      * :end
        * with milliseconds since epoch value
          * [uses this value in the history request](./spec/acceptance/rest/channel_spec.rb#L389)
        * with a Time object value
          * [converts the value to milliseconds since epoch in the hisotry request](./spec/acceptance/rest/channel_spec.rb#L399)
      * when argument start is after end
        * [should raise an exception](./spec/acceptance/rest/channel_spec.rb#L409)
    * #presence
      * [returns a REST Presence object](./spec/acceptance/rest/channel_spec.rb#L419)

### Ably::Rest::Channels
_(see [spec/acceptance/rest/channels_spec.rb](./spec/acceptance/rest/channels_spec.rb))_
  * using JSON protocol
    * using shortcut method #channel on the client object
      * behaves like a channel
        * [returns a channel object](./spec/acceptance/rest/channels_spec.rb#L6)
        * [returns channel object and passes the provided options](./spec/acceptance/rest/channels_spec.rb#L11)
    * using #get method on client#channels
      * behaves like a channel
        * [returns a channel object](./spec/acceptance/rest/channels_spec.rb#L6)
        * [returns channel object and passes the provided options](./spec/acceptance/rest/channels_spec.rb#L11)
    * accessing an existing channel object with different options
      * [overrides the existing channel options and returns the channel object](./spec/acceptance/rest/channels_spec.rb#L39)
    * accessing an existing channel object without specifying any channel options
      * [returns the existing channel without modifying the channel options](./spec/acceptance/rest/channels_spec.rb#L50)
    * using undocumented array accessor [] method on client#channels
      * behaves like a channel
        * [returns a channel object](./spec/acceptance/rest/channels_spec.rb#L6)
        * [returns channel object and passes the provided options](./spec/acceptance/rest/channels_spec.rb#L11)

### Ably::Rest::Client
_(see [spec/acceptance/rest/client_spec.rb](./spec/acceptance/rest/client_spec.rb))_
  * using JSON protocol
    * #initialize
      * with only an API key
        * [uses basic authentication](./spec/acceptance/rest/client_spec.rb#L25)
      * with an explicit string :token
        * [uses token authentication](./spec/acceptance/rest/client_spec.rb#L33)
      * with :use_token_auth set to true
        * [uses token authentication](./spec/acceptance/rest/client_spec.rb#L41)
      * with a :client_id configured
        * [uses token authentication](./spec/acceptance/rest/client_spec.rb#L49)
      * with a non string :client_id
        * [raises an ArgumentError](./spec/acceptance/rest/client_spec.rb#L57)
      * with an invalid wildcard "*" :client_id
        * [raises an exception](./spec/acceptance/rest/client_spec.rb#L63)
      * with an :auth_callback Proc
        * [calls the auth Proc to get a new token](./spec/acceptance/rest/client_spec.rb#L71)
        * [uses token authentication](./spec/acceptance/rest/client_spec.rb#L76)
      * with :default_token_params
        * [overides the default token params (#TO3j11)](./spec/acceptance/rest/client_spec.rb#L90)
      * with an :auth_callback Proc (clientId provided in library options instead of as a token_request param)
        * [correctly sets the clientId on the token](./spec/acceptance/rest/client_spec.rb#L100)
      * with an auth URL
        * [uses token authentication](./spec/acceptance/rest/client_spec.rb#L110)
        * before any REST request
          * [sends an HTTP request to the provided auth URL to get a new token](./spec/acceptance/rest/client_spec.rb#L121)
      * auth headers
        * with basic auth
          * [sends the API key in authentication part of the secure URL (the Authorization: Basic header is not used with the Faraday HTTP library by default)](./spec/acceptance/rest/client_spec.rb#L141)
        * with token auth
          * without specifying protocol
            * [sends the token string over HTTPS in the Authorization Bearer header with Base64 encoding](./spec/acceptance/rest/client_spec.rb#L160)
          * when setting constructor ClientOption :tls to false
            * [sends the token string over HTTP in the Authorization Bearer header with Base64 encoding](./spec/acceptance/rest/client_spec.rb#L170)
    * using tokens
      * when expired
        * [creates a new token automatically when the old token expires](./spec/acceptance/rest/client_spec.rb#L203)
        * with a different client_id in the subsequent token
          * [fails to authenticate and raises an exception](./spec/acceptance/rest/client_spec.rb#L216)
      * when token has not expired
        * [reuses the existing token for every request](./spec/acceptance/rest/client_spec.rb#L227)
    * connection transport
      * defaults
        * for default host
          * [is configured to timeout connection opening in 4 seconds](./spec/acceptance/rest/client_spec.rb#L244)
          * [is configured to timeout connection requests in 15 seconds](./spec/acceptance/rest/client_spec.rb#L248)
        * for the fallback hosts
          * [is configured to timeout connection opening in 4 seconds](./spec/acceptance/rest/client_spec.rb#L254)
          * [is configured to timeout connection requests in 15 seconds](./spec/acceptance/rest/client_spec.rb#L258)
      * with custom http_open_timeout and http_request_timeout options
        * for default host
          * [is configured to use custom open timeout](./spec/acceptance/rest/client_spec.rb#L270)
          * [is configured to use custom request timeout](./spec/acceptance/rest/client_spec.rb#L274)
        * for the fallback hosts
          * [is configured to timeout connection opening in 4 seconds](./spec/acceptance/rest/client_spec.rb#L280)
          * [is configured to timeout connection requests in 15 seconds](./spec/acceptance/rest/client_spec.rb#L284)
    * fallback hosts
      * configured
        * [should make connection attempts to A.ably-realtime.com, B.ably-realtime.com, C.ably-realtime.com, D.ably-realtime.com, E.ably-realtime.com (#RSC15a)](./spec/acceptance/rest/client_spec.rb#L298)
      * when environment is NOT production (#RSC15b)
        * [does not retry failed requests with fallback hosts when there is a connection error](./spec/acceptance/rest/client_spec.rb#L315)
      * when environment is production
        * and connection times out
          * [tries fallback hosts 3 times (#RSC15b, #RSC15b)](./spec/acceptance/rest/client_spec.rb#L354)
          * and the total request time exeeds 10 seconds
            * [makes no further attempts to any fallback hosts](./spec/acceptance/rest/client_spec.rb#L369)
        * and connection fails
          * [tries fallback hosts 3 times](./spec/acceptance/rest/client_spec.rb#L385)
        * and first request to primary endpoint fails
          * [tries a fallback host, and for the next request tries the primary endpoint again (#RSC15e)](./spec/acceptance/rest/client_spec.rb#L419)
        * and basic authentication fails
          * [does not attempt the fallback hosts as this is an authentication failure](./spec/acceptance/rest/client_spec.rb#L446)
        * and server returns a 50x error
          * [attempts the fallback hosts as this is an authentication failure (#RSC15d)](./spec/acceptance/rest/client_spec.rb#L468)
      * when environment is production and server returns a 50x error
        * with custom fallback hosts provided
          * [attempts the fallback hosts as this is an authentication failure (#RSC15b, #RSC15a, #TO3k6)](./spec/acceptance/rest/client_spec.rb#L517)
        * with an empty array of fallback hosts provided (#RSC15b, #RSC15a, #TO3k6)
          * [does not attempt the fallback hosts as this is an authentication failure](./spec/acceptance/rest/client_spec.rb#L530)
        * using a local web-server
          * and timing out the primary host
            * with request timeout less than max_retry_duration
              * [tries one of the fallback hosts (#RSC15d)](./spec/acceptance/rest/client_spec.rb#L586)
            * with request timeout less than max_retry_duration
              * [tries one of the fallback hosts (#RSC15d)](./spec/acceptance/rest/client_spec.rb#L608)
          * and failing the primary host
            * [tries one of the fallback hosts](./spec/acceptance/rest/client_spec.rb#L652)
      * when environment is not production and server returns a 50x error
        * with custom fallback hosts provided (#RSC15b, #TO3k6)
          * [attempts the fallback hosts as this is not an authentication failure](./spec/acceptance/rest/client_spec.rb#L702)
        * with an empty array of fallback hosts provided (#RSC15b, #TO3k6)
          * [does not attempt the fallback hosts as this is an authentication failure](./spec/acceptance/rest/client_spec.rb#L715)
        * with fallback_hosts_use_default: true (#RSC15b, #TO3k7)
          * [attempts the default fallback hosts as this is an authentication failure](./spec/acceptance/rest/client_spec.rb#L740)
    * with a custom host
      * that does not exist
        * [fails immediately and raises a Faraday Error](./spec/acceptance/rest/client_spec.rb#L756)
        * fallback hosts
          * [are never used](./spec/acceptance/rest/client_spec.rb#L777)
      * that times out
        * [fails immediately and raises a Faraday Error](./spec/acceptance/rest/client_spec.rb#L792)
        * fallback hosts
          * [are never used](./spec/acceptance/rest/client_spec.rb#L805)
    * HTTP configuration options
      * [is frozen](./spec/acceptance/rest/client_spec.rb#L862)
      * defaults
        * [#http_open_timeout is 4s](./spec/acceptance/rest/client_spec.rb#L817)
        * [#http_request_timeout is 15s](./spec/acceptance/rest/client_spec.rb#L821)
        * [#http_max_retry_count is 3](./spec/acceptance/rest/client_spec.rb#L825)
        * [#http_max_retry_duration is 10s](./spec/acceptance/rest/client_spec.rb#L829)
      * configured
        * [#http_open_timeout uses provided value](./spec/acceptance/rest/client_spec.rb#L845)
        * [#http_request_timeout uses provided value](./spec/acceptance/rest/client_spec.rb#L849)
        * [#http_max_retry_count uses provided value](./spec/acceptance/rest/client_spec.rb#L853)
        * [#http_max_retry_duration uses provided value](./spec/acceptance/rest/client_spec.rb#L857)
    * #auth
      * [is provides access to the Auth object](./spec/acceptance/rest/client_spec.rb#L873)
      * [configures the Auth object with all ClientOptions passed to client in the initializer](./spec/acceptance/rest/client_spec.rb#L877)
    * version headers
      * with variant none
        * [sends a protocol version and lib version header (#G4, #RSC7a, #RSC7b)](./spec/acceptance/rest/client_spec.rb#L911)
      * with variant foo
        * [sends a protocol version and lib version header (#G4, #RSC7a, #RSC7b)](./spec/acceptance/rest/client_spec.rb#L911)
    * #request (#RSC19*)
      * get
        * [returns an HttpPaginatedResponse object](./spec/acceptance/rest/client_spec.rb#L924)
        * 404 request to invalid URL
          * [returns an object with 404 status code and error message](./spec/acceptance/rest/client_spec.rb#L931)
        * paged results
          * [provides paging](./spec/acceptance/rest/client_spec.rb#L943)

### Ably::Models::MessageEncoders
_(see [spec/acceptance/rest/encoders_spec.rb](./spec/acceptance/rest/encoders_spec.rb))_
  * with binary transport protocol
    * without encryption
      * with UTF-8 data
        * [does not apply any encoding](./spec/acceptance/rest/encoders_spec.rb#L41)
      * with binary data
        * [does not apply any encoding](./spec/acceptance/rest/encoders_spec.rb#L52)
      * with JSON data
        * [stringifies the JSON and sets the encoding attribute to "json"](./spec/acceptance/rest/encoders_spec.rb#L63)
    * with encryption
      * with UTF-8 data
        * [applies utf-8 and cipher encoding and sets the encoding attribute to "utf-8/cipher+aes-128-cbc"](./spec/acceptance/rest/encoders_spec.rb#L78)
      * with binary data
        * [applies cipher encoding and sets the encoding attribute to "cipher+aes-128-cbc"](./spec/acceptance/rest/encoders_spec.rb#L89)
      * with JSON data
        * [applies json, utf-8 and cipher encoding and sets the encoding attribute to "json/utf-8/cipher+aes-128-cbc"](./spec/acceptance/rest/encoders_spec.rb#L100)
  * with text transport protocol
    * without encryption
      * with UTF-8 data
        * [does not apply any encoding](./spec/acceptance/rest/encoders_spec.rb#L117)
      * with binary data
        * [applies a base64 encoding and sets the encoding attribute to "base64"](./spec/acceptance/rest/encoders_spec.rb#L128)
      * with JSON data
        * [stringifies the JSON and sets the encoding attribute to "json"](./spec/acceptance/rest/encoders_spec.rb#L139)
    * with encryption
      * with UTF-8 data
        * [applies utf-8, cipher and base64 encodings and sets the encoding attribute to "utf-8/cipher+aes-128-cbc/base64"](./spec/acceptance/rest/encoders_spec.rb#L154)
      * with binary data
        * [applies cipher and base64 encoding and sets the encoding attribute to "cipher+aes-128-cbc/base64"](./spec/acceptance/rest/encoders_spec.rb#L165)
      * with JSON data
        * [applies json, utf-8, cipher and base64 encoding and sets the encoding attribute to "json/utf-8/cipher+aes-128-cbc/base64"](./spec/acceptance/rest/encoders_spec.rb#L176)

### Ably::Rest::Channel messages
_(see [spec/acceptance/rest/message_spec.rb](./spec/acceptance/rest/message_spec.rb))_
  * using JSON protocol
    * publishing with an ASCII_8BIT message name
      * [is converted into UTF_8](./spec/acceptance/rest/message_spec.rb#L18)
    * with supported data payload content type
      * JSON Object (Hash)
        * [is encoded and decoded to the same hash](./spec/acceptance/rest/message_spec.rb#L30)
      * JSON Array
        * [is encoded and decoded to the same Array](./spec/acceptance/rest/message_spec.rb#L39)
      * String
        * [is encoded and decoded to the same Array](./spec/acceptance/rest/message_spec.rb#L48)
      * Binary
        * [is encoded and decoded to the same Array](./spec/acceptance/rest/message_spec.rb#L57)
    * with supported extra payload content type (#RSL1h, #RSL6a2)
      * JSON Object (Hash)
        * PENDING: *[is encoded and decoded to the same hash](./spec/acceptance/rest/message_spec.rb#L68)*
      * JSON Array
        * PENDING: *[is encoded and decoded to the same Array](./spec/acceptance/rest/message_spec.rb#L78)*
      * nil
        * [is encoded and decoded to the same Array](./spec/acceptance/rest/message_spec.rb#L86)
    * with unsupported data payload content type
      * Integer
        * [is raises an UnsupportedDataType 40011 exception](./spec/acceptance/rest/message_spec.rb#L97)
      * Float
        * [is raises an UnsupportedDataType 40011 exception](./spec/acceptance/rest/message_spec.rb#L105)
      * Boolean
        * [is raises an UnsupportedDataType 40011 exception](./spec/acceptance/rest/message_spec.rb#L113)
      * False
        * [is raises an UnsupportedDataType 40011 exception](./spec/acceptance/rest/message_spec.rb#L121)
    * encryption and encoding
      * with #publish and #history
        * with AES-128-CBC using crypto-data-128.json fixtures (#RTL7d)
          * item 0 with encrypted encoding utf-8/cipher+aes-128-cbc/base64
            * behaves like an Ably encrypter and decrypter
              * [encrypts message automatically when published (#RTL7d)](./spec/acceptance/rest/message_spec.rb#L166)
              * [sends and retrieves messages that are encrypted & decrypted by the Ably library (#RTL7d)](./spec/acceptance/rest/message_spec.rb#L181)
          * item 1 with encrypted encoding cipher+aes-128-cbc/base64
            * behaves like an Ably encrypter and decrypter
              * [encrypts message automatically when published (#RTL7d)](./spec/acceptance/rest/message_spec.rb#L166)
              * [sends and retrieves messages that are encrypted & decrypted by the Ably library (#RTL7d)](./spec/acceptance/rest/message_spec.rb#L181)
          * item 2 with encrypted encoding json/utf-8/cipher+aes-128-cbc/base64
            * behaves like an Ably encrypter and decrypter
              * [encrypts message automatically when published (#RTL7d)](./spec/acceptance/rest/message_spec.rb#L166)
              * [sends and retrieves messages that are encrypted & decrypted by the Ably library (#RTL7d)](./spec/acceptance/rest/message_spec.rb#L181)
          * item 3 with encrypted encoding json/utf-8/cipher+aes-128-cbc/base64
            * behaves like an Ably encrypter and decrypter
              * [encrypts message automatically when published (#RTL7d)](./spec/acceptance/rest/message_spec.rb#L166)
              * [sends and retrieves messages that are encrypted & decrypted by the Ably library (#RTL7d)](./spec/acceptance/rest/message_spec.rb#L181)
        * with AES-256-CBC using crypto-data-256.json fixtures (#RTL7d)
          * item 0 with encrypted encoding utf-8/cipher+aes-256-cbc/base64
            * behaves like an Ably encrypter and decrypter
              * [encrypts message automatically when published (#RTL7d)](./spec/acceptance/rest/message_spec.rb#L166)
              * [sends and retrieves messages that are encrypted & decrypted by the Ably library (#RTL7d)](./spec/acceptance/rest/message_spec.rb#L181)
          * item 1 with encrypted encoding cipher+aes-256-cbc/base64
            * behaves like an Ably encrypter and decrypter
              * [encrypts message automatically when published (#RTL7d)](./spec/acceptance/rest/message_spec.rb#L166)
              * [sends and retrieves messages that are encrypted & decrypted by the Ably library (#RTL7d)](./spec/acceptance/rest/message_spec.rb#L181)
          * item 2 with encrypted encoding json/utf-8/cipher+aes-256-cbc/base64
            * behaves like an Ably encrypter and decrypter
              * [encrypts message automatically when published (#RTL7d)](./spec/acceptance/rest/message_spec.rb#L166)
              * [sends and retrieves messages that are encrypted & decrypted by the Ably library (#RTL7d)](./spec/acceptance/rest/message_spec.rb#L181)
          * item 3 with encrypted encoding json/utf-8/cipher+aes-256-cbc/base64
            * behaves like an Ably encrypter and decrypter
              * [encrypts message automatically when published (#RTL7d)](./spec/acceptance/rest/message_spec.rb#L166)
              * [sends and retrieves messages that are encrypted & decrypted by the Ably library (#RTL7d)](./spec/acceptance/rest/message_spec.rb#L181)
        * when publishing lots of messages
          * [encrypts on #publish and decrypts on #history](./spec/acceptance/rest/message_spec.rb#L214)
        * when retrieving #history with a different protocol
          * [delivers a String ASCII-8BIT payload to the receiver](./spec/acceptance/rest/message_spec.rb#L241)
          * [delivers a String UTF-8 payload to the receiver](./spec/acceptance/rest/message_spec.rb#L241)
          * [delivers a Hash payload to the receiver](./spec/acceptance/rest/message_spec.rb#L241)
        * when publishing on an unencrypted channel and retrieving with #history on an encrypted channel
          * [does not attempt to decrypt the message](./spec/acceptance/rest/message_spec.rb#L257)
        * when publishing on an encrypted channel and retrieving with #history on an unencrypted channel
          * [retrieves the message that remains encrypted with an encrypted encoding attribute (#RTL7e)](./spec/acceptance/rest/message_spec.rb#L278)
          * [logs a Cipher exception (#RTL7e)](./spec/acceptance/rest/message_spec.rb#L284)
        * publishing on an encrypted channel and retrieving #history with a different algorithm on another client (#RTL7e)
          * [retrieves the message that remains encrypted with an encrypted encoding attribute (#RTL7e)](./spec/acceptance/rest/message_spec.rb#L305)
          * [logs a Cipher exception (#RTL7e)](./spec/acceptance/rest/message_spec.rb#L311)
        * publishing on an encrypted channel and subscribing with a different key on another client
          * [retrieves the message that remains encrypted with an encrypted encoding attribute](./spec/acceptance/rest/message_spec.rb#L332)
          * [logs a Cipher exception](./spec/acceptance/rest/message_spec.rb#L338)

### Ably::Rest::Presence
_(see [spec/acceptance/rest/presence_spec.rb](./spec/acceptance/rest/presence_spec.rb))_
  * using JSON protocol
    * tested against presence fixture data set up in test app
      * #get
        * [returns current members on the channel with their action set to :present](./spec/acceptance/rest/presence_spec.rb#L41)
        * with :limit option
          * [returns a paged response limiting number of members per page](./spec/acceptance/rest/presence_spec.rb#L57)
        * default :limit
          * [defaults to a limit of 100](./spec/acceptance/rest/presence_spec.rb#L86)
        * with :client_id option
          * [returns a list members filtered by the provided client ID](./spec/acceptance/rest/presence_spec.rb#L95)
        * with :connection_id option
          * [returns a list members filtered by the provided connection ID](./spec/acceptance/rest/presence_spec.rb#L106)
          * [returns a list members filtered by the provided connection ID](./spec/acceptance/rest/presence_spec.rb#L110)
      * #history
        * [returns recent presence activity](./spec/acceptance/rest/presence_spec.rb#L119)
        * default behaviour
          * [uses backwards direction](./spec/acceptance/rest/presence_spec.rb#L134)
        * with options
          * direction: :forwards
            * [returns recent presence activity forwards with most recent history last](./spec/acceptance/rest/presence_spec.rb#L146)
          * direction: :backwards
            * [returns recent presence activity backwards with most recent history first](./spec/acceptance/rest/presence_spec.rb#L161)
    * #history
      * with options
        * limit options
          * default
            * [is set to 100](./spec/acceptance/rest/presence_spec.rb#L206)
          * set to 1000
            * [is passes the limit query param value 1000](./spec/acceptance/rest/presence_spec.rb#L219)
        * with time range options
          * :start
            * with milliseconds since epoch value
              * [uses this value in the history request](./spec/acceptance/rest/presence_spec.rb#L249)
            * with Time object value
              * [converts the value to milliseconds since epoch in the hisotry request](./spec/acceptance/rest/presence_spec.rb#L259)
          * :end
            * with milliseconds since epoch value
              * [uses this value in the history request](./spec/acceptance/rest/presence_spec.rb#L249)
            * with Time object value
              * [converts the value to milliseconds since epoch in the hisotry request](./spec/acceptance/rest/presence_spec.rb#L259)
          * when argument start is after end
            * [should raise an exception](./spec/acceptance/rest/presence_spec.rb#L270)
    * decoding
      * with encoded fixture data
        * #history
          * [decodes encoded and encryped presence fixture data automatically](./spec/acceptance/rest/presence_spec.rb#L289)
        * #get
          * [decodes encoded and encryped presence fixture data automatically](./spec/acceptance/rest/presence_spec.rb#L296)
    * decoding permutations using mocked #history
      * valid decodeable content
        * #get
          * [automaticaly decodes presence messages](./spec/acceptance/rest/presence_spec.rb#L349)
        * #history
          * [automaticaly decodes presence messages](./spec/acceptance/rest/presence_spec.rb#L366)
      * invalid data
        * #get
          * [returns the messages still encoded](./spec/acceptance/rest/presence_spec.rb#L397)
          * [logs a cipher error](./spec/acceptance/rest/presence_spec.rb#L401)
        * #history
          * [returns the messages still encoded](./spec/acceptance/rest/presence_spec.rb#L421)
          * [logs a cipher error](./spec/acceptance/rest/presence_spec.rb#L425)

### Ably::Rest::Client#stats
_(see [spec/acceptance/rest/stats_spec.rb](./spec/acceptance/rest/stats_spec.rb))_
  * using JSON protocol
    * fetching application stats
      * [returns a PaginatedResult object](./spec/acceptance/rest/stats_spec.rb#L54)
      * by minute
        * with no options
          * [uses the minute interval by default](./spec/acceptance/rest/stats_spec.rb#L66)
        * with :from set to last interval and :limit set to 1
          * [retrieves only one stat](./spec/acceptance/rest/stats_spec.rb#L75)
          * [returns zero value for any missing metrics](./spec/acceptance/rest/stats_spec.rb#L79)
          * [returns all aggregated message data](./spec/acceptance/rest/stats_spec.rb#L84)
          * [returns inbound realtime all data](./spec/acceptance/rest/stats_spec.rb#L89)
          * [returns inbound realtime message data](./spec/acceptance/rest/stats_spec.rb#L94)
          * [returns outbound realtime all data](./spec/acceptance/rest/stats_spec.rb#L99)
          * [returns persisted presence all data](./spec/acceptance/rest/stats_spec.rb#L104)
          * [returns connections all data](./spec/acceptance/rest/stats_spec.rb#L109)
          * [returns channels all data](./spec/acceptance/rest/stats_spec.rb#L114)
          * [returns api_requests data](./spec/acceptance/rest/stats_spec.rb#L119)
          * [returns token_requests data](./spec/acceptance/rest/stats_spec.rb#L124)
          * [returns stat objects with #interval_granularity equal to :minute](./spec/acceptance/rest/stats_spec.rb#L129)
          * [returns stat objects with #interval_id matching :start](./spec/acceptance/rest/stats_spec.rb#L133)
          * [returns stat objects with #interval_time matching :start Time](./spec/acceptance/rest/stats_spec.rb#L137)
        * with :start set to first interval, :limit set to 1 and direction :forwards
          * [returns the first interval stats as stats are provided forwards from :start](./spec/acceptance/rest/stats_spec.rb#L147)
          * [returns 3 pages of stats](./spec/acceptance/rest/stats_spec.rb#L151)
        * with :end set to last interval, :limit set to 1 and direction :backwards
          * [returns the 3rd interval stats first as stats are provided backwards from :end](./spec/acceptance/rest/stats_spec.rb#L163)
          * [returns 3 pages of stats](./spec/acceptance/rest/stats_spec.rb#L167)
        * with :end set to last interval and :limit set to 3 to ensure only last years stats are included
          * the REST API
            * [defaults to direction :backwards](./spec/acceptance/rest/stats_spec.rb#L179)
        * with :end set to previous year interval
          * the REST API
            * [defaults to 100 items for pagination](./spec/acceptance/rest/stats_spec.rb#L191)
      * by hour
        * [should aggregate the stats for that period](./spec/acceptance/rest/stats_spec.rb#L215)
      * by day
        * [should aggregate the stats for that period](./spec/acceptance/rest/stats_spec.rb#L215)
      * by month
        * [should aggregate the stats for that period](./spec/acceptance/rest/stats_spec.rb#L215)
      * when argument start is after end
        * [should raise an exception](./spec/acceptance/rest/stats_spec.rb#L227)

### Ably::Rest::Client#time
_(see [spec/acceptance/rest/time_spec.rb](./spec/acceptance/rest/time_spec.rb))_
  * using JSON protocol
    * fetching the service time
      * [should return the service time as a Time object](./spec/acceptance/rest/time_spec.rb#L10)
      * with reconfigured HTTP timeout
        * [should raise a timeout exception](./spec/acceptance/rest/time_spec.rb#L19)

### Ably::Auth
_(see [spec/unit/auth_spec.rb](./spec/unit/auth_spec.rb))_
  * client_id option
    * with nil value
      * [is permitted](./spec/unit/auth_spec.rb#L20)
    * as UTF_8 string
      * [is permitted](./spec/unit/auth_spec.rb#L28)
      * [remains as UTF-8](./spec/unit/auth_spec.rb#L32)
    * as SHIFT_JIS string
      * [gets converted to UTF-8](./spec/unit/auth_spec.rb#L40)
      * [is compatible with original encoding](./spec/unit/auth_spec.rb#L44)
    * as ASCII_8BIT string
      * [gets converted to UTF-8](./spec/unit/auth_spec.rb#L52)
      * [is compatible with original encoding](./spec/unit/auth_spec.rb#L56)
    * as Integer
      * [raises an argument error](./spec/unit/auth_spec.rb#L64)
  * defaults
    * [should default TTL to 1 hour](./spec/unit/auth_spec.rb#L74)
    * [should default capability to all](./spec/unit/auth_spec.rb#L78)
    * [should have defaults for :ttl and :capability](./spec/unit/auth_spec.rb#L82)

### Ably::Logger
_(see [spec/unit/logger_spec.rb](./spec/unit/logger_spec.rb))_
  * [uses the language provided Logger by default](./spec/unit/logger_spec.rb#L15)
  * with a custom Logger
    * with an invalid interface
      * [raises an exception](./spec/unit/logger_spec.rb#L118)
    * with a valid interface
      * [is used](./spec/unit/logger_spec.rb#L137)
  * with blocks
    * [does not call the block unless the log level is met](./spec/unit/logger_spec.rb#L152)
    * with an exception in the logger block
      * [catches the error and continues](./spec/unit/logger_spec.rb#L167)

### Ably::Models::AuthDetails
_(see [spec/unit/models/auth_details_spec.rb](./spec/unit/models/auth_details_spec.rb))_
  * behaves like a model
    * attributes
      * #access_token
        * [retrieves attribute :access_token](./spec/shared/model_behaviour.rb#L15)
    * #==
      * [is true when attributes are the same](./spec/shared/model_behaviour.rb#L41)
      * [is false when attributes are not the same](./spec/shared/model_behaviour.rb#L46)
      * [is false when class type differs](./spec/shared/model_behaviour.rb#L50)
    * is immutable
      * [prevents changes](./spec/shared/model_behaviour.rb#L76)
      * [dups options](./spec/shared/model_behaviour.rb#L80)
  * ==
    * [is true when attributes are the same](./spec/unit/models/auth_details_spec.rb#L17)
    * [is false when attributes are not the same](./spec/unit/models/auth_details_spec.rb#L22)
    * [is false when class type differs](./spec/unit/models/auth_details_spec.rb#L26)

### Ably::Models::ChannelStateChange
_(see [spec/unit/models/channel_state_change_spec.rb](./spec/unit/models/channel_state_change_spec.rb))_
  * #current (#TH1)
    * [is required](./spec/unit/models/channel_state_change_spec.rb#L10)
    * [is an attribute](./spec/unit/models/channel_state_change_spec.rb#L14)
  * #previous (#TH2)
    * [is required](./spec/unit/models/channel_state_change_spec.rb#L20)
    * [is an attribute](./spec/unit/models/channel_state_change_spec.rb#L24)
  * #event (#TH5)
    * [is not required](./spec/unit/models/channel_state_change_spec.rb#L30)
    * [is an attribute](./spec/unit/models/channel_state_change_spec.rb#L34)
  * #reason (#TH3)
    * [is not required](./spec/unit/models/channel_state_change_spec.rb#L40)
    * [is an attribute](./spec/unit/models/channel_state_change_spec.rb#L44)
  * #resumed (#TH4)
    * [is false when ommitted](./spec/unit/models/channel_state_change_spec.rb#L50)
    * [is true when provided](./spec/unit/models/channel_state_change_spec.rb#L54)
  * invalid attributes
    * [raises an argument error](./spec/unit/models/channel_state_change_spec.rb#L60)

### Ably::Models::CipherParams
_(see [spec/unit/models/cipher_params_spec.rb](./spec/unit/models/cipher_params_spec.rb))_
  * :key missing from constructor
    * [raises an exception](./spec/unit/models/cipher_params_spec.rb#L8)
  * #key
    * with :key in constructor
      * as nil
        * [raises an exception](./spec/unit/models/cipher_params_spec.rb#L20)
      * as a base64 encoded string
        * [is a binary representation of the base64 encoded string](./spec/unit/models/cipher_params_spec.rb#L29)
      * as a URL safe base64 encoded string
        * [is a binary representation of the URL safe base64 encoded string](./spec/unit/models/cipher_params_spec.rb#L40)
      * as a binary encoded string
        * [contains the binary string](./spec/unit/models/cipher_params_spec.rb#L48)
      * with an incompatible :key_length constructor param
        * [raises an exception](./spec/unit/models/cipher_params_spec.rb#L58)
      * with an unsupported :key_length for aes-cbc encryption
        * [raises an exception](./spec/unit/models/cipher_params_spec.rb#L67)
      * with an invalid type
        * [raises an exception](./spec/unit/models/cipher_params_spec.rb#L76)
  * with specified params in the constructor
    * #cipher_type
      * [contains the complete algorithm string as an upper case string](./spec/unit/models/cipher_params_spec.rb#L88)
    * #mode
      * [contains the mode](./spec/unit/models/cipher_params_spec.rb#L94)
    * #algorithm
      * [contains the algorithm](./spec/unit/models/cipher_params_spec.rb#L100)
    * #key_length
      * [contains the key_length](./spec/unit/models/cipher_params_spec.rb#L106)
  * with combined param in the constructor
    * #cipher_type
      * [contains the complete algorithm string as an upper case string](./spec/unit/models/cipher_params_spec.rb#L117)
    * #mode
      * [contains the mode](./spec/unit/models/cipher_params_spec.rb#L123)
    * #algorithm
      * [contains the algorithm](./spec/unit/models/cipher_params_spec.rb#L129)
    * #key_length
      * [contains the key_length](./spec/unit/models/cipher_params_spec.rb#L135)

### Ably::Models::ConnectionDetails
_(see [spec/unit/models/connection_details_spec.rb](./spec/unit/models/connection_details_spec.rb))_
  * behaves like a model
    * attributes
      * #client_id
        * [retrieves attribute :client_id](./spec/shared/model_behaviour.rb#L15)
      * #connection_key
        * [retrieves attribute :connection_key](./spec/shared/model_behaviour.rb#L15)
      * #max_message_size
        * [retrieves attribute :max_message_size](./spec/shared/model_behaviour.rb#L15)
      * #max_frame_size
        * [retrieves attribute :max_frame_size](./spec/shared/model_behaviour.rb#L15)
      * #max_inbound_rate
        * [retrieves attribute :max_inbound_rate](./spec/shared/model_behaviour.rb#L15)
    * #==
      * [is true when attributes are the same](./spec/shared/model_behaviour.rb#L41)
      * [is false when attributes are not the same](./spec/shared/model_behaviour.rb#L46)
      * [is false when class type differs](./spec/shared/model_behaviour.rb#L50)
    * is immutable
      * [prevents changes](./spec/shared/model_behaviour.rb#L76)
      * [dups options](./spec/shared/model_behaviour.rb#L80)
  * attributes
    * #connection_state_ttl (#CD2f)
      * [retrieves attribute :connection_state_ttl and converts it from ms to s](./spec/unit/models/connection_details_spec.rb#L20)
    * #max_idle_interval (#CD2h)
      * [retrieves attribute :max_idle_interval and converts it from ms to s](./spec/unit/models/connection_details_spec.rb#L30)
  * ==
    * [is true when attributes are the same](./spec/unit/models/connection_details_spec.rb#L39)
    * [is false when attributes are not the same](./spec/unit/models/connection_details_spec.rb#L44)
    * [is false when class type differs](./spec/unit/models/connection_details_spec.rb#L48)

### Ably::Models::ConnectionStateChange
_(see [spec/unit/models/connection_state_change_spec.rb](./spec/unit/models/connection_state_change_spec.rb))_
  * #current (#TA2)
    * [is required](./spec/unit/models/connection_state_change_spec.rb#L10)
    * [is an attribute](./spec/unit/models/connection_state_change_spec.rb#L14)
  * #previous(#TA2)
    * [is required](./spec/unit/models/connection_state_change_spec.rb#L20)
    * [is an attribute](./spec/unit/models/connection_state_change_spec.rb#L24)
  * #event(#TA5)
    * [is not required](./spec/unit/models/connection_state_change_spec.rb#L30)
    * [is an attribute](./spec/unit/models/connection_state_change_spec.rb#L34)
  * #retry_in (#TA2)
    * [is not required](./spec/unit/models/connection_state_change_spec.rb#L41)
    * [is an attribute](./spec/unit/models/connection_state_change_spec.rb#L45)
  * #reason (#TA3)
    * [is not required](./spec/unit/models/connection_state_change_spec.rb#L51)
    * [is an attribute](./spec/unit/models/connection_state_change_spec.rb#L55)
  * invalid attributes
    * [raises an argument error](./spec/unit/models/connection_state_change_spec.rb#L61)

### Ably::Models::ErrorInfo
_(see [spec/unit/models/error_info_spec.rb](./spec/unit/models/error_info_spec.rb))_
  * behaves like a model
    * attributes
      * #code
        * [retrieves attribute :code](./spec/shared/model_behaviour.rb#L15)
      * #status_code
        * [retrieves attribute :status_code](./spec/shared/model_behaviour.rb#L15)
      * #message
        * [retrieves attribute :message](./spec/shared/model_behaviour.rb#L15)
    * #==
      * [is true when attributes are the same](./spec/shared/model_behaviour.rb#L41)
      * [is false when attributes are not the same](./spec/shared/model_behaviour.rb#L46)
      * [is false when class type differs](./spec/shared/model_behaviour.rb#L50)
    * is immutable
      * [prevents changes](./spec/shared/model_behaviour.rb#L76)
      * [dups options](./spec/shared/model_behaviour.rb#L80)
  * #status
    * [is an alias for #status_code](./spec/unit/models/error_info_spec.rb#L13)

### Ably::Models::HttpPaginatedResponse: #HP1 -> #HP8
_(see [spec/unit/models/http_paginated_result_spec.rb](./spec/unit/models/http_paginated_result_spec.rb))_
  * #items
    * [returns correct length from body](./spec/unit/models/http_paginated_result_spec.rb#L33)
    * [is Enumerable](./spec/unit/models/http_paginated_result_spec.rb#L37)
    * [is iterable](./spec/unit/models/http_paginated_result_spec.rb#L41)
    * [provides [] accessor method](./spec/unit/models/http_paginated_result_spec.rb#L59)
    * [#first gets the first item in page](./spec/unit/models/http_paginated_result_spec.rb#L65)
    * [#last gets the last item in page](./spec/unit/models/http_paginated_result_spec.rb#L69)
    * #each
      * [returns an enumerator](./spec/unit/models/http_paginated_result_spec.rb#L46)
      * [yields each item](./spec/unit/models/http_paginated_result_spec.rb#L50)
  * with non paged http response
    * [is the last page](./spec/unit/models/http_paginated_result_spec.rb#L174)
    * [does not have next page](./spec/unit/models/http_paginated_result_spec.rb#L178)
    * [does not support pagination](./spec/unit/models/http_paginated_result_spec.rb#L182)
    * [returns nil when accessing next page](./spec/unit/models/http_paginated_result_spec.rb#L186)
    * [returns nil when accessing first page](./spec/unit/models/http_paginated_result_spec.rb#L190)
  * with paged http response
    * [has next page](./spec/unit/models/http_paginated_result_spec.rb#L208)
    * [is not the last page](./spec/unit/models/http_paginated_result_spec.rb#L212)
    * [supports pagination](./spec/unit/models/http_paginated_result_spec.rb#L216)
    * accessing next page
      * [returns another HttpPaginatedResponse](./spec/unit/models/http_paginated_result_spec.rb#L244)
      * [retrieves the next page of results](./spec/unit/models/http_paginated_result_spec.rb#L248)
      * [does not have a next page](./spec/unit/models/http_paginated_result_spec.rb#L253)
      * [is the last page](./spec/unit/models/http_paginated_result_spec.rb#L257)
      * [returns nil when trying to access the last page when it is the last page](./spec/unit/models/http_paginated_result_spec.rb#L261)
      * and then first page
        * [returns a HttpPaginatedResponse](./spec/unit/models/http_paginated_result_spec.rb#L272)
        * [retrieves the first page of results](./spec/unit/models/http_paginated_result_spec.rb#L276)
  * response metadata
    * successful response
      * [#success? is true](./spec/unit/models/http_paginated_result_spec.rb#L288)
      * [#status_code reflects status code](./spec/unit/models/http_paginated_result_spec.rb#L292)
      * [#error_code to be empty](./spec/unit/models/http_paginated_result_spec.rb#L296)
      * [#error_message to be empty](./spec/unit/models/http_paginated_result_spec.rb#L300)
      * [#headers to be a hash](./spec/unit/models/http_paginated_result_spec.rb#L304)
    * failed response
      * [#success? is false](./spec/unit/models/http_paginated_result_spec.rb#L313)
      * [#status_code reflects status code](./spec/unit/models/http_paginated_result_spec.rb#L317)
      * [#error_code to be populated](./spec/unit/models/http_paginated_result_spec.rb#L321)
      * [#error_message to be populated](./spec/unit/models/http_paginated_result_spec.rb#L325)
      * [#headers to be present](./spec/unit/models/http_paginated_result_spec.rb#L329)
  * #items Array conversion and nil handling #HP3
    * with Json Array
      * [is an array](./spec/unit/models/http_paginated_result_spec.rb#L344)
    * with Json Object
      * [is an array](./spec/unit/models/http_paginated_result_spec.rb#L354)
    * with empty response
      * [is an array](./spec/unit/models/http_paginated_result_spec.rb#L365)
    * with nil response
      * [is an array](./spec/unit/models/http_paginated_result_spec.rb#L375)

### Ably::Models::MessageEncoders::Base64
_(see [spec/unit/models/message_encoders/base64_spec.rb](./spec/unit/models/message_encoders/base64_spec.rb))_
  * #decode
    * message with base64 payload
      * [decodes base64](./spec/unit/models/message_encoders/base64_spec.rb#L24)
      * [strips the encoding](./spec/unit/models/message_encoders/base64_spec.rb#L28)
    * message with base64 payload before other payloads
      * [decodes base64](./spec/unit/models/message_encoders/base64_spec.rb#L36)
      * [strips the encoding](./spec/unit/models/message_encoders/base64_spec.rb#L40)
    * message with another payload
      * [leaves the message data intact](./spec/unit/models/message_encoders/base64_spec.rb#L48)
      * [leaves the encoding intact](./spec/unit/models/message_encoders/base64_spec.rb#L52)
  * #encode
    * over binary transport
      * message with binary payload
        * [leaves the message data intact as Base64 encoding is not necessary](./spec/unit/models/message_encoders/base64_spec.rb#L69)
        * [leaves the encoding intact](./spec/unit/models/message_encoders/base64_spec.rb#L73)
      * already encoded message with binary payload
        * [leaves the message data intact as Base64 encoding is not necessary](./spec/unit/models/message_encoders/base64_spec.rb#L81)
        * [leaves the encoding intact](./spec/unit/models/message_encoders/base64_spec.rb#L85)
      * message with UTF-8 payload
        * [leaves the data intact](./spec/unit/models/message_encoders/base64_spec.rb#L93)
        * [leaves the encoding intact](./spec/unit/models/message_encoders/base64_spec.rb#L97)
      * message with nil payload
        * [leaves the message data intact](./spec/unit/models/message_encoders/base64_spec.rb#L105)
        * [leaves the encoding intact](./spec/unit/models/message_encoders/base64_spec.rb#L109)
      * message with empty binary string payload
        * [leaves the message data intact](./spec/unit/models/message_encoders/base64_spec.rb#L117)
        * [leaves the encoding intact](./spec/unit/models/message_encoders/base64_spec.rb#L121)
    * over text transport
      * message with binary payload
        * [encodes binary data as base64](./spec/unit/models/message_encoders/base64_spec.rb#L136)
        * [adds the encoding](./spec/unit/models/message_encoders/base64_spec.rb#L140)
      * already encoded message with binary payload
        * [encodes binary data as base64](./spec/unit/models/message_encoders/base64_spec.rb#L148)
        * [adds the encoding](./spec/unit/models/message_encoders/base64_spec.rb#L152)
      * message with UTF-8 payload
        * [leaves the data intact](./spec/unit/models/message_encoders/base64_spec.rb#L160)
        * [leaves the encoding intact](./spec/unit/models/message_encoders/base64_spec.rb#L164)
      * message with nil payload
        * [leaves the message data intact](./spec/unit/models/message_encoders/base64_spec.rb#L172)
        * [leaves the encoding intact](./spec/unit/models/message_encoders/base64_spec.rb#L176)

### Ably::Models::MessageEncoders::Cipher
_(see [spec/unit/models/message_encoders/cipher_spec.rb](./spec/unit/models/message_encoders/cipher_spec.rb))_
  * #decode
    * with channel set up for AES-128-CBC
      * valid cipher data
        * message with cipher payload
          * [decodes cipher](./spec/unit/models/message_encoders/cipher_spec.rb#L32)
          * [strips the encoding](./spec/unit/models/message_encoders/cipher_spec.rb#L36)
        * message with cipher payload before other payloads
          * [decodes cipher](./spec/unit/models/message_encoders/cipher_spec.rb#L44)
          * [strips the encoding](./spec/unit/models/message_encoders/cipher_spec.rb#L48)
        * message with binary payload
          * [decodes cipher](./spec/unit/models/message_encoders/cipher_spec.rb#L56)
          * [strips the encoding](./spec/unit/models/message_encoders/cipher_spec.rb#L60)
          * [returns ASCII_8BIT encoded binary data](./spec/unit/models/message_encoders/cipher_spec.rb#L64)
        * message with another payload
          * [leaves the message data intact](./spec/unit/models/message_encoders/cipher_spec.rb#L72)
          * [leaves the encoding intact](./spec/unit/models/message_encoders/cipher_spec.rb#L76)
      * 256 bit key
        * with invalid channel_option cipher params
          * [raise an exception](./spec/unit/models/message_encoders/cipher_spec.rb#L90)
        * without any configured encryption
          * [raise an exception](./spec/unit/models/message_encoders/cipher_spec.rb#L100)
      * with invalid cipher data
        * [raise an exception](./spec/unit/models/message_encoders/cipher_spec.rb#L110)
    * with AES-256-CBC
      * message with cipher payload
        * [decodes cipher](./spec/unit/models/message_encoders/cipher_spec.rb#L127)
        * [strips the encoding](./spec/unit/models/message_encoders/cipher_spec.rb#L131)
  * #encode
    * with channel set up for AES-128-CBC
      * with encrypted set to true
        * message with string payload
          * [encodes cipher](./spec/unit/models/message_encoders/cipher_spec.rb#L151)
          * [adds the encoding with utf-8](./spec/unit/models/message_encoders/cipher_spec.rb#L156)
        * message with binary payload
          * [encodes cipher](./spec/unit/models/message_encoders/cipher_spec.rb#L164)
          * [adds the encoding without utf-8 prefixed](./spec/unit/models/message_encoders/cipher_spec.rb#L169)
          * [returns ASCII_8BIT encoded binary data](./spec/unit/models/message_encoders/cipher_spec.rb#L173)
        * message with json payload
          * [encodes cipher](./spec/unit/models/message_encoders/cipher_spec.rb#L181)
          * [adds the encoding with utf-8](./spec/unit/models/message_encoders/cipher_spec.rb#L186)
        * message with existing cipher encoding before
          * [leaves message intact as it is already encrypted](./spec/unit/models/message_encoders/cipher_spec.rb#L194)
          * [leaves encoding intact](./spec/unit/models/message_encoders/cipher_spec.rb#L198)
        * with encryption set to to false
          * [leaves message intact as encryption is not enable](./spec/unit/models/message_encoders/cipher_spec.rb#L207)
          * [leaves encoding intact](./spec/unit/models/message_encoders/cipher_spec.rb#L211)
      * channel_option cipher params
        * have invalid key length
          * [raise an exception](./spec/unit/models/message_encoders/cipher_spec.rb#L223)
        * have invalid algorithm
          * [raise an exception](./spec/unit/models/message_encoders/cipher_spec.rb#L230)
        * have missing key
          * [raise an exception](./spec/unit/models/message_encoders/cipher_spec.rb#L237)
    * with AES-256-CBC
      * message with cipher payload
        * [decodes cipher](./spec/unit/models/message_encoders/cipher_spec.rb#L255)
        * [strips the encoding](./spec/unit/models/message_encoders/cipher_spec.rb#L260)

### Ably::Models::MessageEncoders::Json
_(see [spec/unit/models/message_encoders/json_spec.rb](./spec/unit/models/message_encoders/json_spec.rb))_
  * #decode
    * message with json payload
      * [decodes json](./spec/unit/models/message_encoders/json_spec.rb#L24)
      * [strips the encoding](./spec/unit/models/message_encoders/json_spec.rb#L28)
    * message with json payload in camelCase
      * [decodes json](./spec/unit/models/message_encoders/json_spec.rb#L36)
      * [strips the encoding](./spec/unit/models/message_encoders/json_spec.rb#L40)
    * message with json payload before other payloads
      * [decodes json](./spec/unit/models/message_encoders/json_spec.rb#L48)
      * [strips the encoding](./spec/unit/models/message_encoders/json_spec.rb#L52)
    * message with another payload
      * [leaves the message data intact](./spec/unit/models/message_encoders/json_spec.rb#L60)
      * [leaves the encoding intact](./spec/unit/models/message_encoders/json_spec.rb#L64)
  * #encode
    * message with hash payload
      * [encodes hash payload data as json](./spec/unit/models/message_encoders/json_spec.rb#L78)
      * [adds the encoding](./spec/unit/models/message_encoders/json_spec.rb#L82)
    * message with hash payload and underscore case keys
      * [encodes hash payload data as json and leaves underscore case in tact](./spec/unit/models/message_encoders/json_spec.rb#L90)
      * [adds the encoding](./spec/unit/models/message_encoders/json_spec.rb#L94)
    * already encoded message with hash payload
      * [encodes hash payload data as json](./spec/unit/models/message_encoders/json_spec.rb#L102)
      * [adds the encoding](./spec/unit/models/message_encoders/json_spec.rb#L106)
    * message with Array payload
      * [encodes Array payload data as json](./spec/unit/models/message_encoders/json_spec.rb#L114)
      * [adds the encoding](./spec/unit/models/message_encoders/json_spec.rb#L118)
    * message with UTF-8 payload
      * [leaves the message data intact](./spec/unit/models/message_encoders/json_spec.rb#L126)
      * [leaves the encoding intact](./spec/unit/models/message_encoders/json_spec.rb#L130)
    * message with nil payload
      * [leaves the message data intact](./spec/unit/models/message_encoders/json_spec.rb#L138)
      * [leaves the encoding intact](./spec/unit/models/message_encoders/json_spec.rb#L142)
    * message with no data payload
      * [leaves the message data intact](./spec/unit/models/message_encoders/json_spec.rb#L150)
      * [leaves the encoding intact](./spec/unit/models/message_encoders/json_spec.rb#L154)

### Ably::Models::MessageEncoders::Utf8
_(see [spec/unit/models/message_encoders/utf8_spec.rb](./spec/unit/models/message_encoders/utf8_spec.rb))_
  * #decode
    * message with utf8 payload
      * [sets the encoding](./spec/unit/models/message_encoders/utf8_spec.rb#L21)
      * [strips the encoding](./spec/unit/models/message_encoders/utf8_spec.rb#L26)
    * message with utf8 payload before other payloads
      * [sets the encoding](./spec/unit/models/message_encoders/utf8_spec.rb#L34)
      * [strips the encoding](./spec/unit/models/message_encoders/utf8_spec.rb#L39)
    * message with another payload
      * [leaves the message data intact](./spec/unit/models/message_encoders/utf8_spec.rb#L47)
      * [leaves the encoding intact](./spec/unit/models/message_encoders/utf8_spec.rb#L51)

### Ably::Models::Message
_(see [spec/unit/models/message_spec.rb](./spec/unit/models/message_spec.rb))_
  * behaves like a model
    * attributes
      * #id
        * [retrieves attribute :id](./spec/shared/model_behaviour.rb#L15)
      * #name
        * [retrieves attribute :name](./spec/shared/model_behaviour.rb#L15)
      * #client_id
        * [retrieves attribute :client_id](./spec/shared/model_behaviour.rb#L15)
      * #data
        * [retrieves attribute :data](./spec/shared/model_behaviour.rb#L15)
      * #encoding
        * [retrieves attribute :encoding](./spec/shared/model_behaviour.rb#L15)
    * #==
      * [is true when attributes are the same](./spec/shared/model_behaviour.rb#L41)
      * [is false when attributes are not the same](./spec/shared/model_behaviour.rb#L46)
      * [is false when class type differs](./spec/shared/model_behaviour.rb#L50)
    * is immutable
      * [prevents changes](./spec/shared/model_behaviour.rb#L76)
      * [dups options](./spec/shared/model_behaviour.rb#L80)
  * #timestamp
    * [retrieves attribute :timestamp as Time object from ProtocolMessage](./spec/unit/models/message_spec.rb#L22)
  * #extras (#TM2i)
    * when missing
      * [is nil](./spec/unit/models/message_spec.rb#L33)
    * when a string
      * [raises an exception](./spec/unit/models/message_spec.rb#L40)
    * when a Hash
      * [contains a Hash Json object](./spec/unit/models/message_spec.rb#L47)
    * when a Json Array
      * [contains a Json Array object](./spec/unit/models/message_spec.rb#L54)
  * #connection_id attribute
    * when this model has a connectionId attribute
      * but no protocol message
        * [uses the model value](./spec/unit/models/message_spec.rb#L69)
      * with a protocol message with a different connectionId
        * [uses the model value](./spec/unit/models/message_spec.rb#L77)
    * when this model has no connectionId attribute
      * and no protocol message
        * [uses the model value](./spec/unit/models/message_spec.rb#L87)
      * with a protocol message with a connectionId
        * [uses the model value](./spec/unit/models/message_spec.rb#L95)
  * initialized with
    * :name
      * as UTF_8 string
        * [is permitted](./spec/unit/models/message_spec.rb#L122)
        * [remains as UTF-8](./spec/unit/models/message_spec.rb#L126)
      * as SHIFT_JIS string
        * [gets converted to UTF-8](./spec/unit/models/message_spec.rb#L134)
        * [is compatible with original encoding](./spec/unit/models/message_spec.rb#L138)
      * as ASCII_8BIT string
        * [gets converted to UTF-8](./spec/unit/models/message_spec.rb#L146)
        * [is compatible with original encoding](./spec/unit/models/message_spec.rb#L150)
      * as Integer
        * [raises an argument error](./spec/unit/models/message_spec.rb#L158)
      * as Nil
        * [is permitted](./spec/unit/models/message_spec.rb#L166)
    * :client_id
      * as UTF_8 string
        * [is permitted](./spec/unit/models/message_spec.rb#L122)
        * [remains as UTF-8](./spec/unit/models/message_spec.rb#L126)
      * as SHIFT_JIS string
        * [gets converted to UTF-8](./spec/unit/models/message_spec.rb#L134)
        * [is compatible with original encoding](./spec/unit/models/message_spec.rb#L138)
      * as ASCII_8BIT string
        * [gets converted to UTF-8](./spec/unit/models/message_spec.rb#L146)
        * [is compatible with original encoding](./spec/unit/models/message_spec.rb#L150)
      * as Integer
        * [raises an argument error](./spec/unit/models/message_spec.rb#L158)
      * as Nil
        * [is permitted](./spec/unit/models/message_spec.rb#L166)
    * :encoding
      * as UTF_8 string
        * [is permitted](./spec/unit/models/message_spec.rb#L122)
        * [remains as UTF-8](./spec/unit/models/message_spec.rb#L126)
      * as SHIFT_JIS string
        * [gets converted to UTF-8](./spec/unit/models/message_spec.rb#L134)
        * [is compatible with original encoding](./spec/unit/models/message_spec.rb#L138)
      * as ASCII_8BIT string
        * [gets converted to UTF-8](./spec/unit/models/message_spec.rb#L146)
        * [is compatible with original encoding](./spec/unit/models/message_spec.rb#L150)
      * as Integer
        * [raises an argument error](./spec/unit/models/message_spec.rb#L158)
      * as Nil
        * [is permitted](./spec/unit/models/message_spec.rb#L166)
  * #from_encoded (#TM3)
    * with no encoding
      * [returns a message object](./spec/unit/models/message_spec.rb#L422)
      * with a block
        * [does not call the block](./spec/unit/models/message_spec.rb#L430)
    * with an encoding
      * [returns a message object](./spec/unit/models/message_spec.rb#L447)
    * with a custom encoding
      * [returns a message object with the residual incompatible transforms left in the encoding property](./spec/unit/models/message_spec.rb#L462)
    * with a Cipher encoding
      * [returns a message object with the residual incompatible transforms left in the encoding property](./spec/unit/models/message_spec.rb#L481)
    * with invalid Cipher encoding
      * without a block
        * [raises an exception](./spec/unit/models/message_spec.rb#L499)
      * with a block
        * [calls the block with the exception](./spec/unit/models/message_spec.rb#L505)
  * #from_encoded_array (#TM3)
    * with no encoding
      * [returns an Array of message objects](./spec/unit/models/message_spec.rb#L524)

### Ably::Models::PaginatedResult
_(see [spec/unit/models/paginated_result_spec.rb](./spec/unit/models/paginated_result_spec.rb))_
  * #items
    * [returns correct length from body](./spec/unit/models/paginated_result_spec.rb#L31)
    * [is Enumerable](./spec/unit/models/paginated_result_spec.rb#L35)
    * [is iterable](./spec/unit/models/paginated_result_spec.rb#L39)
    * [provides [] accessor method](./spec/unit/models/paginated_result_spec.rb#L57)
    * [#first gets the first item in page](./spec/unit/models/paginated_result_spec.rb#L63)
    * [#last gets the last item in page](./spec/unit/models/paginated_result_spec.rb#L67)
    * #each
      * [returns an enumerator](./spec/unit/models/paginated_result_spec.rb#L44)
      * [yields each item](./spec/unit/models/paginated_result_spec.rb#L48)
  * with non paged http response
    * [is the last page](./spec/unit/models/paginated_result_spec.rb#L172)
    * [does not have next page](./spec/unit/models/paginated_result_spec.rb#L176)
    * [does not support pagination](./spec/unit/models/paginated_result_spec.rb#L180)
    * [returns nil when accessing next page](./spec/unit/models/paginated_result_spec.rb#L184)
    * [returns nil when accessing first page](./spec/unit/models/paginated_result_spec.rb#L188)
  * with paged http response
    * [has next page](./spec/unit/models/paginated_result_spec.rb#L206)
    * [is not the last page](./spec/unit/models/paginated_result_spec.rb#L210)
    * [supports pagination](./spec/unit/models/paginated_result_spec.rb#L214)
    * accessing next page
      * [returns another PaginatedResult](./spec/unit/models/paginated_result_spec.rb#L242)
      * [retrieves the next page of results](./spec/unit/models/paginated_result_spec.rb#L246)
      * [does not have a next page](./spec/unit/models/paginated_result_spec.rb#L251)
      * [is the last page](./spec/unit/models/paginated_result_spec.rb#L255)
      * [returns nil when trying to access the last page when it is the last page](./spec/unit/models/paginated_result_spec.rb#L259)
      * and then first page
        * [returns a PaginatedResult](./spec/unit/models/paginated_result_spec.rb#L270)
        * [retrieves the first page of results](./spec/unit/models/paginated_result_spec.rb#L274)

### Ably::Models::PresenceMessage
_(see [spec/unit/models/presence_message_spec.rb](./spec/unit/models/presence_message_spec.rb))_
  * behaves like a model
    * attributes
      * #id
        * [retrieves attribute :id](./spec/shared/model_behaviour.rb#L15)
      * #client_id
        * [retrieves attribute :client_id](./spec/shared/model_behaviour.rb#L15)
      * #data
        * [retrieves attribute :data](./spec/shared/model_behaviour.rb#L15)
      * #encoding
        * [retrieves attribute :encoding](./spec/shared/model_behaviour.rb#L15)
    * #==
      * [is true when attributes are the same](./spec/shared/model_behaviour.rb#L41)
      * [is false when attributes are not the same](./spec/shared/model_behaviour.rb#L46)
      * [is false when class type differs](./spec/shared/model_behaviour.rb#L50)
    * is immutable
      * [prevents changes](./spec/shared/model_behaviour.rb#L76)
      * [dups options](./spec/shared/model_behaviour.rb#L80)
  * #connection_id attribute
    * when this model has a connectionId attribute
      * but no protocol message
        * [uses the model value](./spec/unit/models/presence_message_spec.rb#L25)
      * with a protocol message with a different connectionId
        * [uses the model value](./spec/unit/models/presence_message_spec.rb#L33)
    * when this model has no connectionId attribute
      * and no protocol message
        * [uses the model value](./spec/unit/models/presence_message_spec.rb#L43)
      * with a protocol message with a connectionId
        * [uses the model value](./spec/unit/models/presence_message_spec.rb#L51)
  * #member_key attribute
    * [is string in format connection_id:client_id](./spec/unit/models/presence_message_spec.rb#L61)
    * with the same client id across multiple connections
      * [is unique](./spec/unit/models/presence_message_spec.rb#L69)
    * with a single connection and different client_ids
      * [is unique](./spec/unit/models/presence_message_spec.rb#L78)
  * #timestamp
    * [retrieves attribute :timestamp as a Time object from ProtocolMessage](./spec/unit/models/presence_message_spec.rb#L86)
  * initialized with
    * :client_id
      * as UTF_8 string
        * [is permitted](./spec/unit/models/presence_message_spec.rb#L138)
        * [remains as UTF-8](./spec/unit/models/presence_message_spec.rb#L142)
      * as SHIFT_JIS string
        * [gets converted to UTF-8](./spec/unit/models/presence_message_spec.rb#L150)
        * [is compatible with original encoding](./spec/unit/models/presence_message_spec.rb#L154)
      * as ASCII_8BIT string
        * [gets converted to UTF-8](./spec/unit/models/presence_message_spec.rb#L162)
        * [is compatible with original encoding](./spec/unit/models/presence_message_spec.rb#L166)
      * as Integer
        * [raises an argument error](./spec/unit/models/presence_message_spec.rb#L174)
      * as Nil
        * [is permitted](./spec/unit/models/presence_message_spec.rb#L182)
    * :connection_id
      * as UTF_8 string
        * [is permitted](./spec/unit/models/presence_message_spec.rb#L138)
        * [remains as UTF-8](./spec/unit/models/presence_message_spec.rb#L142)
      * as SHIFT_JIS string
        * [gets converted to UTF-8](./spec/unit/models/presence_message_spec.rb#L150)
        * [is compatible with original encoding](./spec/unit/models/presence_message_spec.rb#L154)
      * as ASCII_8BIT string
        * [gets converted to UTF-8](./spec/unit/models/presence_message_spec.rb#L162)
        * [is compatible with original encoding](./spec/unit/models/presence_message_spec.rb#L166)
      * as Integer
        * [raises an argument error](./spec/unit/models/presence_message_spec.rb#L174)
      * as Nil
        * [is permitted](./spec/unit/models/presence_message_spec.rb#L182)
    * :encoding
      * as UTF_8 string
        * [is permitted](./spec/unit/models/presence_message_spec.rb#L138)
        * [remains as UTF-8](./spec/unit/models/presence_message_spec.rb#L142)
      * as SHIFT_JIS string
        * [gets converted to UTF-8](./spec/unit/models/presence_message_spec.rb#L150)
        * [is compatible with original encoding](./spec/unit/models/presence_message_spec.rb#L154)
      * as ASCII_8BIT string
        * [gets converted to UTF-8](./spec/unit/models/presence_message_spec.rb#L162)
        * [is compatible with original encoding](./spec/unit/models/presence_message_spec.rb#L166)
      * as Integer
        * [raises an argument error](./spec/unit/models/presence_message_spec.rb#L174)
      * as Nil
        * [is permitted](./spec/unit/models/presence_message_spec.rb#L182)
  * #from_encoded (#TP4)
    * with no encoding
      * [returns a presence message object](./spec/unit/models/presence_message_spec.rb#L395)
      * with a block
        * [does not call the block](./spec/unit/models/presence_message_spec.rb#L403)
    * with an encoding
      * [returns a presence message object](./spec/unit/models/presence_message_spec.rb#L420)
    * with a custom encoding
      * [returns a presence message object with the residual incompatible transforms left in the encoding property](./spec/unit/models/presence_message_spec.rb#L435)
    * with a Cipher encoding
      * [returns a presence message object with the residual incompatible transforms left in the encoding property](./spec/unit/models/presence_message_spec.rb#L454)
    * with invalid Cipher encoding
      * without a block
        * [raises an exception](./spec/unit/models/presence_message_spec.rb#L471)
      * with a block
        * [calls the block with the exception](./spec/unit/models/presence_message_spec.rb#L477)
  * #from_encoded_array (#TP4)
    * with no encoding
      * [returns an Array of presence message objects](./spec/unit/models/presence_message_spec.rb#L496)
  * #shallow_clone
    * with inherited attributes from ProtocolMessage
      * [creates a duplicate of the message without any ProtocolMessage dependency](./spec/unit/models/presence_message_spec.rb#L516)
    * with embedded attributes for all fields
      * [creates a duplicate of the message without any ProtocolMessage dependency](./spec/unit/models/presence_message_spec.rb#L530)
    * with new attributes passed in to the method
      * [creates a duplicate of the message without any ProtocolMessage dependency](./spec/unit/models/presence_message_spec.rb#L546)
      * with an invalid ProtocolMessage (missing an ID)
        * [allows an ID to be passed in to the shallow clone that takes precedence](./spec/unit/models/presence_message_spec.rb#L558)
      * with mixing of cases
        * [resolves case issues and can use camelCase or snake_case](./spec/unit/models/presence_message_spec.rb#L565)

### Ably::Models::ProtocolMessage
_(see [spec/unit/models/protocol_message_spec.rb](./spec/unit/models/protocol_message_spec.rb))_
  * behaves like a model
    * attributes
      * #id
        * [retrieves attribute :id](./spec/shared/model_behaviour.rb#L15)
      * #channel
        * [retrieves attribute :channel](./spec/shared/model_behaviour.rb#L15)
      * #channel_serial
        * [retrieves attribute :channel_serial](./spec/shared/model_behaviour.rb#L15)
      * #connection_id
        * [retrieves attribute :connection_id](./spec/shared/model_behaviour.rb#L15)
      * #connection_key
        * [retrieves attribute :connection_key](./spec/shared/model_behaviour.rb#L15)
    * #==
      * [is true when attributes are the same](./spec/shared/model_behaviour.rb#L41)
      * [is false when attributes are not the same](./spec/shared/model_behaviour.rb#L46)
      * [is false when class type differs](./spec/shared/model_behaviour.rb#L50)
    * is immutable
      * [prevents changes](./spec/shared/model_behaviour.rb#L76)
      * [dups options](./spec/shared/model_behaviour.rb#L80)
  * attributes
    * #timestamp
      * [retrieves attribute :timestamp as Time object](./spec/unit/models/protocol_message_spec.rb#L75)
    * #count
      * when missing
        * [is 1](./spec/unit/models/protocol_message_spec.rb#L84)
      * when non numeric
        * [is 1](./spec/unit/models/protocol_message_spec.rb#L91)
      * when greater than 1
        * [is the value of count](./spec/unit/models/protocol_message_spec.rb#L98)
    * #message_serial
      * [converts :msg_serial to an Integer](./spec/unit/models/protocol_message_spec.rb#L106)
    * #has_message_serial?
      * without msg_serial
        * [returns false](./spec/unit/models/protocol_message_spec.rb#L116)
      * with msg_serial
        * [returns true](./spec/unit/models/protocol_message_spec.rb#L124)
    * #connection_serial
      * [converts :connection_serial to an Integer](./spec/unit/models/protocol_message_spec.rb#L132)
    * #flags (#TR4i)
      * when nil
        * [is zero](./spec/unit/models/protocol_message_spec.rb#L142)
      * when numeric
        * [is an Integer](./spec/unit/models/protocol_message_spec.rb#L150)
      * when presence flag present
        * [#has_presence_flag? is true](./spec/unit/models/protocol_message_spec.rb#L158)
        * [#has_channel_resumed_flag? is false](./spec/unit/models/protocol_message_spec.rb#L162)
      * when channel resumed flag present
        * [#has_channel_resumed_flag? is true](./spec/unit/models/protocol_message_spec.rb#L170)
        * [#has_presence_flag? is false](./spec/unit/models/protocol_message_spec.rb#L174)
      * when channel resumed and presence flags present
        * [#has_channel_resumed_flag? is true](./spec/unit/models/protocol_message_spec.rb#L182)
        * [#has_presence_flag? is true](./spec/unit/models/protocol_message_spec.rb#L186)
      * when has another future flag
        * [#has_presence_flag? is false](./spec/unit/models/protocol_message_spec.rb#L194)
        * [#has_backlog_flag? is true](./spec/unit/models/protocol_message_spec.rb#L198)
    * #has_connection_serial?
      * without connection_serial
        * [returns false](./spec/unit/models/protocol_message_spec.rb#L208)
      * with connection_serial
        * [returns true](./spec/unit/models/protocol_message_spec.rb#L216)
    * #serial
      * with underlying msg_serial
        * [converts :msg_serial to an Integer](./spec/unit/models/protocol_message_spec.rb#L225)
      * with underlying connection_serial
        * [converts :connection_serial to an Integer](./spec/unit/models/protocol_message_spec.rb#L233)
      * with underlying connection_serial and msg_serial
        * [prefers connection_serial and converts :connection_serial to an Integer](./spec/unit/models/protocol_message_spec.rb#L241)
    * #has_serial?
      * without msg_serial or connection_serial
        * [returns false](./spec/unit/models/protocol_message_spec.rb#L252)
      * with msg_serial
        * [returns true](./spec/unit/models/protocol_message_spec.rb#L260)
      * with connection_serial
        * [returns true](./spec/unit/models/protocol_message_spec.rb#L268)
    * #error
      * with no error attribute
        * [returns nil](./spec/unit/models/protocol_message_spec.rb#L278)
      * with nil error
        * [returns nil](./spec/unit/models/protocol_message_spec.rb#L286)
      * with error
        * [returns a valid ErrorInfo object](./spec/unit/models/protocol_message_spec.rb#L294)
    * #messages (#TR4k)
      * [contains Message objects](./spec/unit/models/protocol_message_spec.rb#L304)
    * #presence (#TR4l)
      * [contains PresenceMessage objects](./spec/unit/models/protocol_message_spec.rb#L314)
    * #connection_details (#TR4o)
      * with a JSON value
        * [contains a ConnectionDetails object](./spec/unit/models/protocol_message_spec.rb#L327)
        * [contains the attributes from the JSON connectionDetails](./spec/unit/models/protocol_message_spec.rb#L331)
      * without a JSON value
        * [contains an empty ConnectionDetails object](./spec/unit/models/protocol_message_spec.rb#L340)
    * #auth (#TR4p)
      * with a JSON value
        * [contains a AuthDetails object](./spec/unit/models/protocol_message_spec.rb#L354)
        * [contains the attributes from the JSON auth details](./spec/unit/models/protocol_message_spec.rb#L358)
      * without a JSON value
        * [contains an empty AuthDetails object](./spec/unit/models/protocol_message_spec.rb#L366)
    * #connection_key (#TR4e)
      * existing only in #connection_details.connection_key
        * [is returned](./spec/unit/models/protocol_message_spec.rb#L377)
      * existing in both #connection_key and #connection_details.connection_key
        * [returns #connection_details.connection_key as #connection_key will be deprecated > 0.8](./spec/unit/models/protocol_message_spec.rb#L385)

### Ably::Models::Stats
_(see [spec/unit/models/stats_spec.rb](./spec/unit/models/stats_spec.rb))_
  * #all stats
    * [returns a MessageTypes object](./spec/unit/models/stats_spec.rb#L17)
    * [returns value for message counts](./spec/unit/models/stats_spec.rb#L21)
    * [returns value for all data transferred](./spec/unit/models/stats_spec.rb#L25)
    * [returns zero for empty values](./spec/unit/models/stats_spec.rb#L29)
    * [raises an exception for unknown attributes](./spec/unit/models/stats_spec.rb#L33)
    * #all
      * [is a MessageCount object](./spec/unit/models/stats_spec.rb#L39)
    * #presence
      * [is a MessageCount object](./spec/unit/models/stats_spec.rb#L39)
    * #messages
      * [is a MessageCount object](./spec/unit/models/stats_spec.rb#L39)
  * #persisted stats
    * [returns a MessageTypes object](./spec/unit/models/stats_spec.rb#L17)
    * [returns value for message counts](./spec/unit/models/stats_spec.rb#L21)
    * [returns value for all data transferred](./spec/unit/models/stats_spec.rb#L25)
    * [returns zero for empty values](./spec/unit/models/stats_spec.rb#L29)
    * [raises an exception for unknown attributes](./spec/unit/models/stats_spec.rb#L33)
    * #all
      * [is a MessageCount object](./spec/unit/models/stats_spec.rb#L39)
    * #presence
      * [is a MessageCount object](./spec/unit/models/stats_spec.rb#L39)
    * #messages
      * [is a MessageCount object](./spec/unit/models/stats_spec.rb#L39)
  * #inbound stats
    * [returns a MessageTraffic object](./spec/unit/models/stats_spec.rb#L59)
    * [returns value for realtime message counts](./spec/unit/models/stats_spec.rb#L63)
    * [returns value for all presence data](./spec/unit/models/stats_spec.rb#L67)
    * [raises an exception for unknown attributes](./spec/unit/models/stats_spec.rb#L71)
    * #realtime
      * [is a MessageTypes object](./spec/unit/models/stats_spec.rb#L77)
    * #rest
      * [is a MessageTypes object](./spec/unit/models/stats_spec.rb#L77)
    * #webhook
      * [is a MessageTypes object](./spec/unit/models/stats_spec.rb#L77)
    * #all
      * [is a MessageTypes object](./spec/unit/models/stats_spec.rb#L77)
  * #outbound stats
    * [returns a MessageTraffic object](./spec/unit/models/stats_spec.rb#L59)
    * [returns value for realtime message counts](./spec/unit/models/stats_spec.rb#L63)
    * [returns value for all presence data](./spec/unit/models/stats_spec.rb#L67)
    * [raises an exception for unknown attributes](./spec/unit/models/stats_spec.rb#L71)
    * #realtime
      * [is a MessageTypes object](./spec/unit/models/stats_spec.rb#L77)
    * #rest
      * [is a MessageTypes object](./spec/unit/models/stats_spec.rb#L77)
    * #webhook
      * [is a MessageTypes object](./spec/unit/models/stats_spec.rb#L77)
    * #all
      * [is a MessageTypes object](./spec/unit/models/stats_spec.rb#L77)
  * #connections stats
    * [returns a ConnectionTypes object](./spec/unit/models/stats_spec.rb#L91)
    * [returns value for tls opened counts](./spec/unit/models/stats_spec.rb#L95)
    * [returns value for all peak connections](./spec/unit/models/stats_spec.rb#L99)
    * [returns zero for empty values](./spec/unit/models/stats_spec.rb#L103)
    * [raises an exception for unknown attributes](./spec/unit/models/stats_spec.rb#L107)
    * #tls
      * [is a ResourceCount object](./spec/unit/models/stats_spec.rb#L113)
    * #plain
      * [is a ResourceCount object](./spec/unit/models/stats_spec.rb#L113)
    * #all
      * [is a ResourceCount object](./spec/unit/models/stats_spec.rb#L113)
  * #channels stats
    * [returns a ResourceCount object](./spec/unit/models/stats_spec.rb#L126)
    * [returns value for opened counts](./spec/unit/models/stats_spec.rb#L130)
    * [returns value for peak channels](./spec/unit/models/stats_spec.rb#L134)
    * [returns zero for empty values](./spec/unit/models/stats_spec.rb#L138)
    * [raises an exception for unknown attributes](./spec/unit/models/stats_spec.rb#L142)
    * #opened
      * [is a Integer object](./spec/unit/models/stats_spec.rb#L148)
    * #peak
      * [is a Integer object](./spec/unit/models/stats_spec.rb#L148)
    * #mean
      * [is a Integer object](./spec/unit/models/stats_spec.rb#L148)
    * #min
      * [is a Integer object](./spec/unit/models/stats_spec.rb#L148)
    * #refused
      * [is a Integer object](./spec/unit/models/stats_spec.rb#L148)
  * #api_requests stats
    * [returns a RequestCount object](./spec/unit/models/stats_spec.rb#L164)
    * [returns value for succeeded](./spec/unit/models/stats_spec.rb#L168)
    * [returns value for failed](./spec/unit/models/stats_spec.rb#L172)
    * [raises an exception for unknown attributes](./spec/unit/models/stats_spec.rb#L176)
    * #succeeded
      * [is a Integer object](./spec/unit/models/stats_spec.rb#L182)
    * #failed
      * [is a Integer object](./spec/unit/models/stats_spec.rb#L182)
    * #refused
      * [is a Integer object](./spec/unit/models/stats_spec.rb#L182)
  * #token_requests stats
    * [returns a RequestCount object](./spec/unit/models/stats_spec.rb#L164)
    * [returns value for succeeded](./spec/unit/models/stats_spec.rb#L168)
    * [returns value for failed](./spec/unit/models/stats_spec.rb#L172)
    * [raises an exception for unknown attributes](./spec/unit/models/stats_spec.rb#L176)
    * #succeeded
      * [is a Integer object](./spec/unit/models/stats_spec.rb#L182)
    * #failed
      * [is a Integer object](./spec/unit/models/stats_spec.rb#L182)
    * #refused
      * [is a Integer object](./spec/unit/models/stats_spec.rb#L182)
  * #interval_granularity
    * [returns the granularity of the interval_id](./spec/unit/models/stats_spec.rb#L193)
  * #interval_time
    * [returns a Time object representing the start of the interval](./spec/unit/models/stats_spec.rb#L201)
  * class methods
    * #to_interval_id
      * when time zone of time argument is UTC
        * [converts time 2014-02-03:05:06 with granularity :month into 2014-02](./spec/unit/models/stats_spec.rb#L209)
        * [converts time 2014-02-03:05:06 with granularity :day into 2014-02-03](./spec/unit/models/stats_spec.rb#L213)
        * [converts time 2014-02-03:05:06 with granularity :hour into 2014-02-03:05](./spec/unit/models/stats_spec.rb#L217)
        * [converts time 2014-02-03:05:06 with granularity :minute into 2014-02-03:05:06](./spec/unit/models/stats_spec.rb#L221)
        * [fails with invalid granularity](./spec/unit/models/stats_spec.rb#L225)
        * [fails with invalid time](./spec/unit/models/stats_spec.rb#L229)
      * when time zone of time argument is +02:00
        * [converts time 2014-02-03:06 with granularity :hour into 2014-02-03:04 at UTC +00:00](./spec/unit/models/stats_spec.rb#L235)
    * #from_interval_id
      * [converts a month interval_id 2014-02 into a Time object in UTC 0](./spec/unit/models/stats_spec.rb#L242)
      * [converts a day interval_id 2014-02-03 into a Time object in UTC 0](./spec/unit/models/stats_spec.rb#L247)
      * [converts an hour interval_id 2014-02-03:05 into a Time object in UTC 0](./spec/unit/models/stats_spec.rb#L252)
      * [converts a minute interval_id 2014-02-03:05:06 into a Time object in UTC 0](./spec/unit/models/stats_spec.rb#L257)
      * [fails with an invalid interval_id 14-20](./spec/unit/models/stats_spec.rb#L262)
    * #granularity_from_interval_id
      * [returns a :month interval_id for 2014-02](./spec/unit/models/stats_spec.rb#L268)
      * [returns a :day interval_id for 2014-02-03](./spec/unit/models/stats_spec.rb#L272)
      * [returns a :hour interval_id for 2014-02-03:05](./spec/unit/models/stats_spec.rb#L276)
      * [returns a :minute interval_id for 2014-02-03:05:06](./spec/unit/models/stats_spec.rb#L280)
      * [fails with an invalid interval_id 14-20](./spec/unit/models/stats_spec.rb#L284)

### Ably::Models::TokenDetails
_(see [spec/unit/models/token_details_spec.rb](./spec/unit/models/token_details_spec.rb))_
  * behaves like a model
    * attributes
      * #token
        * [retrieves attribute :token](./spec/shared/model_behaviour.rb#L15)
      * #key_name
        * [retrieves attribute :key_name](./spec/shared/model_behaviour.rb#L15)
      * #client_id
        * [retrieves attribute :client_id](./spec/shared/model_behaviour.rb#L15)
    * #==
      * [is true when attributes are the same](./spec/shared/model_behaviour.rb#L41)
      * [is false when attributes are not the same](./spec/shared/model_behaviour.rb#L46)
      * [is false when class type differs](./spec/shared/model_behaviour.rb#L50)
    * is immutable
      * [prevents changes](./spec/shared/model_behaviour.rb#L76)
      * [dups options](./spec/shared/model_behaviour.rb#L80)
  * attributes
    * #capability
      * [retrieves attribute :capability as parsed JSON](./spec/unit/models/token_details_spec.rb#L21)
    * 
      * #issued with :issued option as milliseconds in constructor
        * [retrieves attribute :issued as Time](./spec/unit/models/token_details_spec.rb#L32)
      * #issued with :issued option as a Time in constructor
        * [retrieves attribute :issued as Time](./spec/unit/models/token_details_spec.rb#L41)
      * #issued when converted to JSON
        * [is in milliseconds](./spec/unit/models/token_details_spec.rb#L50)
      * #expires with :expires option as milliseconds in constructor
        * [retrieves attribute :expires as Time](./spec/unit/models/token_details_spec.rb#L32)
      * #expires with :expires option as a Time in constructor
        * [retrieves attribute :expires as Time](./spec/unit/models/token_details_spec.rb#L41)
      * #expires when converted to JSON
        * [is in milliseconds](./spec/unit/models/token_details_spec.rb#L50)
    * #expired?
      * once grace period buffer has passed
        * [is true](./spec/unit/models/token_details_spec.rb#L63)
      * within grace period buffer
        * [is false](./spec/unit/models/token_details_spec.rb#L71)
      * when expires is not available (i.e. string tokens)
        * [is always false](./spec/unit/models/token_details_spec.rb#L79)
  * ==
    * [is true when attributes are the same](./spec/unit/models/token_details_spec.rb#L89)
    * [is false when attributes are not the same](./spec/unit/models/token_details_spec.rb#L94)
    * [is false when class type differs](./spec/unit/models/token_details_spec.rb#L98)
  * to_json
    * with all attributes and values
      * [returns all attributes](./spec/unit/models/token_details_spec.rb#L130)
    * with only a token string
      * [returns populated attributes](./spec/unit/models/token_details_spec.rb#L143)
  * from_json (#TD7)
    * with Ruby idiomatic Hash object
      * [returns a valid TokenDetails object](./spec/unit/models/token_details_spec.rb#L169)
    * with JSON-like object
      * [returns a valid TokenDetails object](./spec/unit/models/token_details_spec.rb#L192)
    * with JSON string
      * [returns a valid TokenDetails object](./spec/unit/models/token_details_spec.rb#L214)

### Ably::Models::TokenRequest
_(see [spec/unit/models/token_request_spec.rb](./spec/unit/models/token_request_spec.rb))_
  * behaves like a model
    * attributes
      * #key_name
        * [retrieves attribute :key_name](./spec/shared/model_behaviour.rb#L15)
      * #client_id
        * [retrieves attribute :client_id](./spec/shared/model_behaviour.rb#L15)
      * #nonce
        * [retrieves attribute :nonce](./spec/shared/model_behaviour.rb#L15)
      * #mac
        * [retrieves attribute :mac](./spec/shared/model_behaviour.rb#L15)
    * #==
      * [is true when attributes are the same](./spec/shared/model_behaviour.rb#L41)
      * [is false when attributes are not the same](./spec/shared/model_behaviour.rb#L46)
      * [is false when class type differs](./spec/shared/model_behaviour.rb#L50)
    * is immutable
      * [prevents changes](./spec/shared/model_behaviour.rb#L76)
      * [dups options](./spec/shared/model_behaviour.rb#L80)
  * attributes
    * #capability
      * [retrieves attribute :capability as parsed JSON](./spec/unit/models/token_request_spec.rb#L18)
    * #timestamp
      * with :timestamp option as milliseconds in constructor
        * [retrieves attribute :timestamp as Time](./spec/unit/models/token_request_spec.rb#L29)
      * with :timestamp option as Time in constructor
        * [retrieves attribute :timestamp as Time](./spec/unit/models/token_request_spec.rb#L38)
      * when converted to JSON
        * [is in milliseconds since epoch](./spec/unit/models/token_request_spec.rb#L47)
    * #ttl
      * with :ttl option as milliseconds in constructor
        * [retrieves attribute :ttl as seconds](./spec/unit/models/token_request_spec.rb#L59)
      * when converted to JSON
        * [is in milliseconds since epoch](./spec/unit/models/token_request_spec.rb#L68)
  * ==
    * [is true when attributes are the same](./spec/unit/models/token_request_spec.rb#L78)
    * [is false when attributes are not the same](./spec/unit/models/token_request_spec.rb#L83)
    * [is false when class type differs](./spec/unit/models/token_request_spec.rb#L87)
  * from_json (#TE6)
    * with Ruby idiomatic Hash object
      * [returns a valid TokenRequest object](./spec/unit/models/token_request_spec.rb#L130)
    * with JSON-like object
      * [returns a valid TokenRequest object](./spec/unit/models/token_request_spec.rb#L152)
    * with JSON string
      * [returns a valid TokenRequest object](./spec/unit/models/token_request_spec.rb#L174)

### Ably::Modules::EventEmitter
_(see [spec/unit/modules/event_emitter_spec.rb](./spec/unit/modules/event_emitter_spec.rb))_
  * #emit event fan out
    * [should emit an event for any number of subscribers](./spec/unit/modules/event_emitter_spec.rb#L21)
    * [sends only messages to matching event names](./spec/unit/modules/event_emitter_spec.rb#L30)
    * #on subscribe to multiple events
      * [with the same block](./spec/unit/modules/event_emitter_spec.rb#L62)
    * event callback changes within the callback block
      * when new event callbacks are added
        * [is unaffected and processes the prior event callbacks once (#RTE6b)](./spec/unit/modules/event_emitter_spec.rb#L86)
        * [adds them for the next emitted event (#RTE6b)](./spec/unit/modules/event_emitter_spec.rb#L92)
      * when callbacks are removed
        * [is unaffected and processes the prior event callbacks once (#RTE6b)](./spec/unit/modules/event_emitter_spec.rb#L113)
        * [removes them for the next emitted event (#RTE6b)](./spec/unit/modules/event_emitter_spec.rb#L118)
  * #on (#RTE3)
    * with event specified
      * [calls the block every time an event is emitted only](./spec/unit/modules/event_emitter_spec.rb#L132)
      * [catches exceptions in the provided block, logs the error and continues](./spec/unit/modules/event_emitter_spec.rb#L139)
    * with no event specified
      * [calls the block every time an event is emitted only](./spec/unit/modules/event_emitter_spec.rb#L149)
      * [catches exceptions in the provided block, logs the error and continues](./spec/unit/modules/event_emitter_spec.rb#L156)
  * #once (#RTE4)
    * with event specified
      * [calls the block the first time an event is emitted only](./spec/unit/modules/event_emitter_spec.rb#L182)
      * [does not remove other blocks after it is called](./spec/unit/modules/event_emitter_spec.rb#L189)
      * [catches exceptions in the provided block, logs the error and continues](./spec/unit/modules/event_emitter_spec.rb#L197)
    * with no event specified
      * [calls the block the first time an event is emitted only](./spec/unit/modules/event_emitter_spec.rb#L207)
      * [does not remove other blocks after it is called](./spec/unit/modules/event_emitter_spec.rb#L214)
      * [catches exceptions in the provided block, logs the error and continues](./spec/unit/modules/event_emitter_spec.rb#L222)
  * #unsafe_once
    * [calls the block the first time an event is emitted only](./spec/unit/modules/event_emitter_spec.rb#L233)
    * [does not catch exceptions in provided blocks](./spec/unit/modules/event_emitter_spec.rb#L240)
  * #off
    * with event specified in on handler
      * with event names as arguments
        * [deletes matching callbacks when a block is provided](./spec/unit/modules/event_emitter_spec.rb#L259)
        * [deletes all matching callbacks when a block is not provided](./spec/unit/modules/event_emitter_spec.rb#L264)
        * [continues if the block does not exist](./spec/unit/modules/event_emitter_spec.rb#L269)
      * without any event names
        * [deletes all matching callbacks](./spec/unit/modules/event_emitter_spec.rb#L276)
        * [deletes all callbacks if not block given](./spec/unit/modules/event_emitter_spec.rb#L281)
    * when on callback is configured for all events
      * with event names as arguments
        * [does not remove the all events callback when a block is provided](./spec/unit/modules/event_emitter_spec.rb#L298)
        * [does not remove the all events callback when a block is not provided](./spec/unit/modules/event_emitter_spec.rb#L303)
        * [does not remove the all events callback when the block does not match](./spec/unit/modules/event_emitter_spec.rb#L308)
      * without any event names
        * [deletes all matching callbacks](./spec/unit/modules/event_emitter_spec.rb#L315)
        * [deletes all callbacks if not block given](./spec/unit/modules/event_emitter_spec.rb#L320)
    * with unsafe_on subscribers
      * [does not deregister them](./spec/unit/modules/event_emitter_spec.rb#L336)
    * with unsafe_once subscribers
      * [does not deregister them](./spec/unit/modules/event_emitter_spec.rb#L351)
  * #unsafe_off
    * with unsafe_on subscribers
      * [deregisters them](./spec/unit/modules/event_emitter_spec.rb#L370)
    * with unsafe_once subscribers
      * [deregister them](./spec/unit/modules/event_emitter_spec.rb#L385)
    * with on subscribers
      * [does not deregister them](./spec/unit/modules/event_emitter_spec.rb#L400)
    * with once subscribers
      * [does not deregister them](./spec/unit/modules/event_emitter_spec.rb#L415)

### Ably::Modules::StateEmitter
_(see [spec/unit/modules/state_emitter_spec.rb](./spec/unit/modules/state_emitter_spec.rb))_
  * [#state returns current state](./spec/unit/modules/state_emitter_spec.rb#L28)
  * [#state= sets current state](./spec/unit/modules/state_emitter_spec.rb#L32)
  * [#change_state sets current state](./spec/unit/modules/state_emitter_spec.rb#L36)
  * #change_state with arguments
    * [passes the arguments through to the executed callback](./spec/unit/modules/state_emitter_spec.rb#L44)
  * #state?
    * [returns true if state matches](./spec/unit/modules/state_emitter_spec.rb#L55)
    * [returns false if state does not match](./spec/unit/modules/state_emitter_spec.rb#L59)
    * and convenience predicates for states
      * [returns true for #initializing? if state matches](./spec/unit/modules/state_emitter_spec.rb#L64)
      * [returns false for #connecting? if state does not match](./spec/unit/modules/state_emitter_spec.rb#L68)

### Ably::Realtime::Channel
_(see [spec/unit/realtime/channel_spec.rb](./spec/unit/realtime/channel_spec.rb))_
  * #initializer
    * as UTF_8 string
      * [is permitted](./spec/unit/realtime/channel_spec.rb#L19)
      * [remains as UTF-8](./spec/unit/realtime/channel_spec.rb#L23)
    * as SHIFT_JIS string
      * [gets converted to UTF-8](./spec/unit/realtime/channel_spec.rb#L31)
      * [is compatible with original encoding](./spec/unit/realtime/channel_spec.rb#L35)
    * as ASCII_8BIT string
      * [gets converted to UTF-8](./spec/unit/realtime/channel_spec.rb#L43)
      * [is compatible with original encoding](./spec/unit/realtime/channel_spec.rb#L47)
    * as Integer
      * [raises an argument error](./spec/unit/realtime/channel_spec.rb#L55)
    * as Nil
      * [raises an argument error](./spec/unit/realtime/channel_spec.rb#L63)
  * #publish name argument
    * as UTF_8 string
      * [is permitted](./spec/unit/realtime/channel_spec.rb#L81)
    * as SHIFT_JIS string
      * [is permitted](./spec/unit/realtime/channel_spec.rb#L89)
    * as ASCII_8BIT string
      * [is permitted](./spec/unit/realtime/channel_spec.rb#L97)
    * as Integer
      * [raises an argument error](./spec/unit/realtime/channel_spec.rb#L105)
    * as Nil
      * [is permitted](./spec/unit/realtime/channel_spec.rb#L113)
  * callbacks
    * [are supported for valid STATE events](./spec/unit/realtime/channel_spec.rb#L120)
    * [fail with unacceptable STATE event names](./spec/unit/realtime/channel_spec.rb#L126)
  * subscriptions
    * #subscribe
      * [without a block raises an invalid ArgumentError](./spec/unit/realtime/channel_spec.rb#L168)
      * [with no event name specified subscribes the provided block to all events](./spec/unit/realtime/channel_spec.rb#L172)
      * [with a single event name subscribes that block to matching events](./spec/unit/realtime/channel_spec.rb#L178)
      * [with a multiple event name arguments subscribes that block to all of those event names](./spec/unit/realtime/channel_spec.rb#L185)
      * [with a multiple duplicate event name arguments subscribes that block to all of those unique event names once](./spec/unit/realtime/channel_spec.rb#L197)
    * #unsubscribe
      * [with no event name specified unsubscribes that block from all events](./spec/unit/realtime/channel_spec.rb#L214)
      * [with a single event name argument unsubscribes the provided block with the matching event name](./spec/unit/realtime/channel_spec.rb#L220)
      * [with multiple event name arguments unsubscribes each of those matching event names with the provided block](./spec/unit/realtime/channel_spec.rb#L226)
      * [with a non-matching event name argument has no effect](./spec/unit/realtime/channel_spec.rb#L232)
      * [with no block argument unsubscribes all blocks for the event name argument](./spec/unit/realtime/channel_spec.rb#L238)

### Ably::Realtime::Channels
_(see [spec/unit/realtime/channels_spec.rb](./spec/unit/realtime/channels_spec.rb))_
  * creating channels
    * [[] creates a channel](./spec/unit/realtime/channels_spec.rb#L43)
    * #get
      * [creates a channel if it does not exist](./spec/unit/realtime/channels_spec.rb#L14)
      * when an existing channel exists
        * [will reuse a channel object if it exists](./spec/unit/realtime/channels_spec.rb#L20)
        * [will update the options on the channel if provided](./spec/unit/realtime/channels_spec.rb#L26)
        * [will leave the options intact on the channel if not provided](./spec/unit/realtime/channels_spec.rb#L34)
  * #fetch
    * [retrieves a channel if it exists](./spec/unit/realtime/channels_spec.rb#L50)
    * [calls the block if channel is missing](./spec/unit/realtime/channels_spec.rb#L55)
  * destroying channels
    * [#release detaches and then releases the channel resources](./spec/unit/realtime/channels_spec.rb#L63)
  * is Enumerable
    * [allows enumeration](./spec/unit/realtime/channels_spec.rb#L80)
    * [provides #length](./spec/unit/realtime/channels_spec.rb#L96)
    * #each
      * [returns an enumerator](./spec/unit/realtime/channels_spec.rb#L85)
      * [yields each channel](./spec/unit/realtime/channels_spec.rb#L89)

### Ably::Realtime::Client
_(see [spec/unit/realtime/client_spec.rb](./spec/unit/realtime/client_spec.rb))_
  * behaves like a client initializer
    * with invalid arguments
      * empty hash
        * [raises an exception](./spec/shared/client_initializer_behaviour.rb#L28)
      * nil
        * [raises an exception](./spec/shared/client_initializer_behaviour.rb#L36)
      * key: "invalid"
        * [raises an exception](./spec/shared/client_initializer_behaviour.rb#L44)
      * key: "invalid:asdad"
        * [raises an exception](./spec/shared/client_initializer_behaviour.rb#L52)
      * key and key_name
        * [raises an exception](./spec/shared/client_initializer_behaviour.rb#L60)
      * key and key_secret
        * [raises an exception](./spec/shared/client_initializer_behaviour.rb#L68)
      * client_id as only option
        * [requires a valid key](./spec/shared/client_initializer_behaviour.rb#L76)
    * with valid arguments
      * key only
        * [connects to the Ably service](./spec/shared/client_initializer_behaviour.rb#L87)
        * [uses basic auth](./spec/shared/client_initializer_behaviour.rb#L91)
      * with a string key instead of options hash
        * [sets the key](./spec/shared/client_initializer_behaviour.rb#L111)
        * [sets the key_name](./spec/shared/client_initializer_behaviour.rb#L115)
        * [sets the key_secret](./spec/shared/client_initializer_behaviour.rb#L119)
        * [uses basic auth](./spec/shared/client_initializer_behaviour.rb#L123)
      * with a string token key instead of options hash
        * [sets the token](./spec/shared/client_initializer_behaviour.rb#L135)
      * with token
        * [sets the token](./spec/shared/client_initializer_behaviour.rb#L143)
      * with token_details
        * [sets the token](./spec/shared/client_initializer_behaviour.rb#L151)
      * with token_params
        * [configures default_token_params](./spec/shared/client_initializer_behaviour.rb#L159)
      * endpoint
        * [defaults to production](./spec/shared/client_initializer_behaviour.rb#L170)
        * with environment option
          * [uses an alternate endpoint](./spec/shared/client_initializer_behaviour.rb#L177)
        * with rest_host option
          * PENDING: *[uses an alternate endpoint for REST clients](./spec/shared/client_initializer_behaviour.rb#L185)*
        * with realtime_host option
          * [uses an alternate endpoint for Realtime clients](./spec/shared/client_initializer_behaviour.rb#L194)
        * with port option and non-TLS connections
          * [uses the custom port for non-TLS requests](./spec/shared/client_initializer_behaviour.rb#L203)
        * with tls_port option and a TLS connection
          * [uses the custom port for TLS requests](./spec/shared/client_initializer_behaviour.rb#L211)
      * tls
        * [defaults to TLS](./spec/shared/client_initializer_behaviour.rb#L234)
        * set to false
          * [uses plain text](./spec/shared/client_initializer_behaviour.rb#L225)
          * [uses HTTP](./spec/shared/client_initializer_behaviour.rb#L229)
      * logger
        * default
          * [uses Ruby Logger](./spec/shared/client_initializer_behaviour.rb#L245)
          * [specifies Logger::WARN log level](./spec/shared/client_initializer_behaviour.rb#L249)
        * with log_level :none
          * [silences all logging with a NilLogger](./spec/shared/client_initializer_behaviour.rb#L257)
        * with custom logger and log_level
          * [uses the custom logger](./spec/shared/client_initializer_behaviour.rb#L275)
          * [sets the custom log level](./spec/shared/client_initializer_behaviour.rb#L279)
    * delegators
      * [delegates :client_id to .auth](./spec/shared/client_initializer_behaviour.rb#L293)
      * [delegates :auth_options to .auth](./spec/shared/client_initializer_behaviour.rb#L298)
  * delegation to the REST Client
    * [passes on the options to the initializer](./spec/unit/realtime/client_spec.rb#L15)
    * for attribute
      * [#environment](./spec/unit/realtime/client_spec.rb#L23)
      * [#use_tls?](./spec/unit/realtime/client_spec.rb#L23)
      * [#log_level](./spec/unit/realtime/client_spec.rb#L23)
      * [#custom_host](./spec/unit/realtime/client_spec.rb#L23)

### Ably::Realtime::Connection
_(see [spec/unit/realtime/connection_spec.rb](./spec/unit/realtime/connection_spec.rb))_
  * callbacks
    * [are supported for valid STATE events](./spec/unit/realtime/connection_spec.rb#L21)
    * [fail with unacceptable STATE event names](./spec/unit/realtime/connection_spec.rb#L27)

### Ably::Realtime::Presence
_(see [spec/unit/realtime/presence_spec.rb](./spec/unit/realtime/presence_spec.rb))_
  * callbacks
    * [are supported for valid STATE events](./spec/unit/realtime/presence_spec.rb#L13)
    * [fail with unacceptable STATE event names](./spec/unit/realtime/presence_spec.rb#L19)
  * subscriptions
    * #subscribe
      * [without a block raises an invalid ArgumentError](./spec/unit/realtime/presence_spec.rb#L63)
      * [with no action specified subscribes the provided block to all action](./spec/unit/realtime/presence_spec.rb#L67)
      * [with a single action argument subscribes that block to matching actions](./spec/unit/realtime/presence_spec.rb#L73)
      * [with a multiple action arguments subscribes that block to all of those actions](./spec/unit/realtime/presence_spec.rb#L80)
      * [with a multiple duplicate action arguments subscribes that block to all of those unique actions once](./spec/unit/realtime/presence_spec.rb#L92)
    * #unsubscribe
      * [with no action specified unsubscribes that block from all events](./spec/unit/realtime/presence_spec.rb#L107)
      * [with a single action argument unsubscribes the provided block with the matching action](./spec/unit/realtime/presence_spec.rb#L113)
      * [with multiple action arguments unsubscribes each of those matching actions with the provided block](./spec/unit/realtime/presence_spec.rb#L119)
      * [with a non-matching action argument has no effect](./spec/unit/realtime/presence_spec.rb#L125)
      * [with no block argument unsubscribes all blocks for the action argument](./spec/unit/realtime/presence_spec.rb#L131)

### Ably::Realtime
_(see [spec/unit/realtime/realtime_spec.rb](./spec/unit/realtime/realtime_spec.rb))_
  * [constructor returns an Ably::Realtime::Client](./spec/unit/realtime/realtime_spec.rb#L6)

### Ably::Models::ProtocolMessage
_(see [spec/unit/realtime/safe_deferrable_spec.rb](./spec/unit/realtime/safe_deferrable_spec.rb))_
  * behaves like a safe Deferrable
    * #errback
      * [adds a callback that is called when #fail is called](./spec/shared/safe_deferrable_behaviour.rb#L15)
      * [catches exceptions in the callback and logs the error to the logger](./spec/shared/safe_deferrable_behaviour.rb#L22)
    * #fail
      * [calls the callbacks defined with #errback, but not the ones added for success #callback](./spec/shared/safe_deferrable_behaviour.rb#L34)
    * #callback
      * [adds a callback that is called when #succed is called](./spec/shared/safe_deferrable_behaviour.rb#L46)
      * [catches exceptions in the callback and logs the error to the logger](./spec/shared/safe_deferrable_behaviour.rb#L53)
    * #succeed
      * [calls the callbacks defined with #callback, but not the ones added for #errback](./spec/shared/safe_deferrable_behaviour.rb#L65)

### Ably::Models::Message
_(see [spec/unit/realtime/safe_deferrable_spec.rb](./spec/unit/realtime/safe_deferrable_spec.rb))_
  * behaves like a safe Deferrable
    * #errback
      * [adds a callback that is called when #fail is called](./spec/shared/safe_deferrable_behaviour.rb#L15)
      * [catches exceptions in the callback and logs the error to the logger](./spec/shared/safe_deferrable_behaviour.rb#L22)
    * #fail
      * [calls the callbacks defined with #errback, but not the ones added for success #callback](./spec/shared/safe_deferrable_behaviour.rb#L34)
    * #callback
      * [adds a callback that is called when #succed is called](./spec/shared/safe_deferrable_behaviour.rb#L46)
      * [catches exceptions in the callback and logs the error to the logger](./spec/shared/safe_deferrable_behaviour.rb#L53)
    * #succeed
      * [calls the callbacks defined with #callback, but not the ones added for #errback](./spec/shared/safe_deferrable_behaviour.rb#L65)

### Ably::Models::PresenceMessage
_(see [spec/unit/realtime/safe_deferrable_spec.rb](./spec/unit/realtime/safe_deferrable_spec.rb))_
  * behaves like a safe Deferrable
    * #errback
      * [adds a callback that is called when #fail is called](./spec/shared/safe_deferrable_behaviour.rb#L15)
      * [catches exceptions in the callback and logs the error to the logger](./spec/shared/safe_deferrable_behaviour.rb#L22)
    * #fail
      * [calls the callbacks defined with #errback, but not the ones added for success #callback](./spec/shared/safe_deferrable_behaviour.rb#L34)
    * #callback
      * [adds a callback that is called when #succed is called](./spec/shared/safe_deferrable_behaviour.rb#L46)
      * [catches exceptions in the callback and logs the error to the logger](./spec/shared/safe_deferrable_behaviour.rb#L53)
    * #succeed
      * [calls the callbacks defined with #callback, but not the ones added for #errback](./spec/shared/safe_deferrable_behaviour.rb#L65)

### Ably::Rest::Channel
_(see [spec/unit/rest/channel_spec.rb](./spec/unit/rest/channel_spec.rb))_
  * #initializer
    * as UTF_8 string
      * [is permitted](./spec/unit/rest/channel_spec.rb#L16)
      * [remains as UTF-8](./spec/unit/rest/channel_spec.rb#L20)
    * as SHIFT_JIS string
      * [gets converted to UTF-8](./spec/unit/rest/channel_spec.rb#L28)
      * [is compatible with original encoding](./spec/unit/rest/channel_spec.rb#L32)
    * as ASCII_8BIT string
      * [gets converted to UTF-8](./spec/unit/rest/channel_spec.rb#L40)
      * [is compatible with original encoding](./spec/unit/rest/channel_spec.rb#L44)
    * as Integer
      * [raises an argument error](./spec/unit/rest/channel_spec.rb#L52)
    * as Nil
      * [raises an argument error](./spec/unit/rest/channel_spec.rb#L60)
  * #publish name argument
    * as UTF_8 string
      * [is permitted](./spec/unit/rest/channel_spec.rb#L72)
    * as SHIFT_JIS string
      * [is permitted](./spec/unit/rest/channel_spec.rb#L80)
    * as ASCII_8BIT string
      * [is permitted](./spec/unit/rest/channel_spec.rb#L88)
    * as Integer
      * [raises an argument error](./spec/unit/rest/channel_spec.rb#L96)

### Ably::Rest::Channels
_(see [spec/unit/rest/channels_spec.rb](./spec/unit/rest/channels_spec.rb))_
  * creating channels
    * [#get creates a channel](./spec/unit/rest/channels_spec.rb#L12)
    * [#get will reuse the channel object](./spec/unit/rest/channels_spec.rb#L17)
    * [[] creates a channel](./spec/unit/rest/channels_spec.rb#L23)
  * #fetch
    * [retrieves a channel if it exists](./spec/unit/rest/channels_spec.rb#L30)
    * [calls the block if channel is missing](./spec/unit/rest/channels_spec.rb#L35)
  * destroying channels
    * [#release releases the channel resoures](./spec/unit/rest/channels_spec.rb#L43)
  * is Enumerable
    * [allows enumeration](./spec/unit/rest/channels_spec.rb#L59)
    * [provides #length](./spec/unit/rest/channels_spec.rb#L75)
    * #each
      * [returns an enumerator](./spec/unit/rest/channels_spec.rb#L64)
      * [yields each channel](./spec/unit/rest/channels_spec.rb#L68)

### Ably::Rest::Client
_(see [spec/unit/rest/client_spec.rb](./spec/unit/rest/client_spec.rb))_
  * behaves like a client initializer
    * with invalid arguments
      * empty hash
        * [raises an exception](./spec/shared/client_initializer_behaviour.rb#L28)
      * nil
        * [raises an exception](./spec/shared/client_initializer_behaviour.rb#L36)
      * key: "invalid"
        * [raises an exception](./spec/shared/client_initializer_behaviour.rb#L44)
      * key: "invalid:asdad"
        * [raises an exception](./spec/shared/client_initializer_behaviour.rb#L52)
      * key and key_name
        * [raises an exception](./spec/shared/client_initializer_behaviour.rb#L60)
      * key and key_secret
        * [raises an exception](./spec/shared/client_initializer_behaviour.rb#L68)
      * client_id as only option
        * [requires a valid key](./spec/shared/client_initializer_behaviour.rb#L76)
    * with valid arguments
      * key only
        * [connects to the Ably service](./spec/shared/client_initializer_behaviour.rb#L87)
        * [uses basic auth](./spec/shared/client_initializer_behaviour.rb#L91)
      * with a string key instead of options hash
        * [sets the key](./spec/shared/client_initializer_behaviour.rb#L111)
        * [sets the key_name](./spec/shared/client_initializer_behaviour.rb#L115)
        * [sets the key_secret](./spec/shared/client_initializer_behaviour.rb#L119)
        * [uses basic auth](./spec/shared/client_initializer_behaviour.rb#L123)
      * with a string token key instead of options hash
        * [sets the token](./spec/shared/client_initializer_behaviour.rb#L135)
      * with token
        * [sets the token](./spec/shared/client_initializer_behaviour.rb#L143)
      * with token_details
        * [sets the token](./spec/shared/client_initializer_behaviour.rb#L151)
      * with token_params
        * [configures default_token_params](./spec/shared/client_initializer_behaviour.rb#L159)
      * endpoint
        * [defaults to production](./spec/shared/client_initializer_behaviour.rb#L170)
        * with environment option
          * [uses an alternate endpoint](./spec/shared/client_initializer_behaviour.rb#L177)
        * with rest_host option
          * [uses an alternate endpoint for REST clients](./spec/shared/client_initializer_behaviour.rb#L185)
        * with realtime_host option
          * PENDING: *[uses an alternate endpoint for Realtime clients](./spec/shared/client_initializer_behaviour.rb#L194)*
        * with port option and non-TLS connections
          * [uses the custom port for non-TLS requests](./spec/shared/client_initializer_behaviour.rb#L203)
        * with tls_port option and a TLS connection
          * [uses the custom port for TLS requests](./spec/shared/client_initializer_behaviour.rb#L211)
      * tls
        * [defaults to TLS](./spec/shared/client_initializer_behaviour.rb#L234)
        * set to false
          * [uses plain text](./spec/shared/client_initializer_behaviour.rb#L225)
          * [uses HTTP](./spec/shared/client_initializer_behaviour.rb#L229)
      * logger
        * default
          * [uses Ruby Logger](./spec/shared/client_initializer_behaviour.rb#L245)
          * [specifies Logger::WARN log level](./spec/shared/client_initializer_behaviour.rb#L249)
        * with log_level :none
          * [silences all logging with a NilLogger](./spec/shared/client_initializer_behaviour.rb#L257)
        * with custom logger and log_level
          * [uses the custom logger](./spec/shared/client_initializer_behaviour.rb#L275)
          * [sets the custom log level](./spec/shared/client_initializer_behaviour.rb#L279)
    * delegators
      * [delegates :client_id to .auth](./spec/shared/client_initializer_behaviour.rb#L293)
      * [delegates :auth_options to .auth](./spec/shared/client_initializer_behaviour.rb#L298)
  * initializer options
    * TLS
      * disabled
        * [fails for any operation with basic auth and attempting to send an API key over a non-secure connection (#RSA1)](./spec/unit/rest/client_spec.rb#L17)
    * :use_token_auth
      * set to false
        * with a key and :tls => false
          * [fails for any operation with basic auth and attempting to send an API key over a non-secure connection](./spec/unit/rest/client_spec.rb#L28)
        * without a key
          * [fails as a key is required if not using token auth](./spec/unit/rest/client_spec.rb#L36)
      * set to true
        * without a key or token
          * [fails as a key is required to issue tokens](./spec/unit/rest/client_spec.rb#L46)

### Ably::Rest
_(see [spec/unit/rest/rest_spec.rb](./spec/unit/rest/rest_spec.rb))_
  * [constructor returns an Ably::Rest::Client](./spec/unit/rest/rest_spec.rb#L7)

### Ably::Util::Crypto
_(see [spec/unit/util/crypto_spec.rb](./spec/unit/util/crypto_spec.rb))_
  * defaults
    * [match other client libraries](./spec/unit/util/crypto_spec.rb#L19)
  * get_default_params
    * with just a :key param
      * [uses the defaults](./spec/unit/util/crypto_spec.rb#L29)
      * [contains the provided key](./spec/unit/util/crypto_spec.rb#L35)
      * [returns a CipherParams object](./spec/unit/util/crypto_spec.rb#L39)
    * without a :key param
      * [raises an exception](./spec/unit/util/crypto_spec.rb#L47)
    * with a base64-encoded :key param
      * [converts the key to binary](./spec/unit/util/crypto_spec.rb#L55)
    * with provided params
      * [overrides the defaults](./spec/unit/util/crypto_spec.rb#L67)
  * encrypts & decrypt
    * [#encrypt encrypts a string](./spec/unit/util/crypto_spec.rb#L79)
    * [#decrypt decrypts a string](./spec/unit/util/crypto_spec.rb#L84)
  * encrypting an empty string
    * [raises an ArgumentError](./spec/unit/util/crypto_spec.rb#L93)
  * using shared client lib fixture data
    * with AES-128-CBC
      * behaves like an Ably encrypter and decrypter (#RTL7d)
        * text payload
          * [encrypts exactly the same binary data as other client libraries](./spec/unit/util/crypto_spec.rb#L116)
          * [decrypts exactly the same binary data as other client libraries](./spec/unit/util/crypto_spec.rb#L120)
    * with AES-256-CBC
      * behaves like an Ably encrypter and decrypter (#RTL7d)
        * text payload
          * [encrypts exactly the same binary data as other client libraries](./spec/unit/util/crypto_spec.rb#L116)
          * [decrypts exactly the same binary data as other client libraries](./spec/unit/util/crypto_spec.rb#L120)

### Ably::Util::PubSub
_(see [spec/unit/util/pub_sub_spec.rb](./spec/unit/util/pub_sub_spec.rb))_
  * event fan out
    * [#publish allows publishing to more than on subscriber](./spec/unit/util/pub_sub_spec.rb#L11)
    * [#publish sends only messages to #subscribe callbacks matching event names](./spec/unit/util/pub_sub_spec.rb#L19)
  * #unsubscribe
    * [deletes matching callbacks](./spec/unit/util/pub_sub_spec.rb#L71)
    * [deletes all callbacks if not block given](./spec/unit/util/pub_sub_spec.rb#L76)
    * [continues if the block does not exist](./spec/unit/util/pub_sub_spec.rb#L81)

  -------

  ## Test summary

  * Passing tests: 1806
  * Pending tests: 12
  * Failing tests: 0
