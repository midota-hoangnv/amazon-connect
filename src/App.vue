<template>
  <div>
    <button id="launch-widget-btn" @click="renderCallButton">
      Call With Us
    </button>

    <div id="container-div" style="width: 400px; height: 800px;"></div>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
  mounted() {
    // initialize the streams api
    this.init();
  },
  methods: {
    renderCallButton() {
      (function(w, d, x, id) {
        var s = d.createElement("script");
        s.src =
          "https://d1nv5i00u1m742.cloudfront.net/amazon-connect-chat-interface-client.js";
        s.async = 1;
        s.id = id;
        d.getElementsByTagName("head")[0].appendChild(s);
        w[x] =
          w[x] ||
          function() {
            (w[x].ac = w[x].ac || []).push(arguments);
          };
      })(
        window,
        document,
        "amazon_connect",
        "fd62e03a-c7bb-40ba-9696-d1c9cecb124a"
      );
      amazon_connect("styles", {
        iconType: "VOICE",
        openChat: { color: "#ffffff", backgroundColor: "#123456" },
        closeChat: { color: "#ffffff", backgroundColor: "#123456" },
      });
      amazon_connect(
        "snippetId",
        "QVFJREFIaDJSeExmZC9mdkhGcGpyYSs4Zm9SMXpVL2FlMFF2aEpVbzNKb3oyNCtZekFGTmJOV2FUMUVBZm81RjJNU3h4L2NSQUFBQWJqQnNCZ2txaGtpRzl3MEJCd2FnWHpCZEFnRUFNRmdHQ1NxR1NJYjNEUUVIQVRBZUJnbGdoa2dCWlFNRUFTNHdFUVFNQnlBb1pPZGdZUGU1bXIrREFnRVFnQ3RvU1VkZ2l3OHUyVnBid2R0UkxlVjEzYlcrV1NlY1FIWXJiS0s0TDMybnpmdk4ySSsrenhPbWJmeis6OlI1b09wc2xkaVVhUVRrdUtqbFV6d0hxUEk5VlU4M2JEWW5XdDlBRnRRdkMxREdJRlZEVVgreHlsejNJR3VHR1JPNm1sWWlSVW56MlVXMHNFZmZkbFlKMWRCdjM5OVl0ODBXUUlhbHlnaXVLQWEvMVNFYisxT3I1dmt0a0xtTFEvYXgzR0cwK2hYTUUrbFdsa2xPYVd5TlQ5Z290M3ZxWT0="
      );
      amazon_connect("supportedMessagingContentTypes", [
        "text/plain",
        "text/markdown",
      ]);
      amazon_connect("customLaunchBehavior", {
        skipIconButtonAndAutoLaunch: true,
        alwaysHideWidgetButton: true,
      });
    },
    init() {
      const containerDiv = document.getElementById("container-div");
      const instanceURL = "https://valqua.my.connect.aws/ccp-v2/";
      // initialize the ccp
      connect.core.initCCP(containerDiv, {
        ccpUrl: instanceURL, // REQUIRED
        // region: 'us-east-1',
        loginPopup: true,
        loginPopupAutoClose: true,
        softphone: {
          allowFramedSoftphone: true,
        },
        pageOptions: {
          enableAudioDeviceSettings: true,
          enablePhoneTypeSettings: true,
        },
      });
      // When an agent is logs in
      connect.agent((agent) => {
        // gather information about the agent
        const agentName = agent.getName();
        const agentContacts = agent.getContacts();
        const agentPermissions = agent.getPermissions();
        agent.onStateChange(function(agentStateChange) {
          console.log(
            "******",
            `Subscribe a method to be called when the agent's state changes`
          );
          console.log("ooo", "******", agentStateChange);
        });
        console.log(
          "******",
          "Subscribe a method to be called when the agent is initialized"
        );
        console.log(
          "ooo",
          "******",
          agent,
          agentName,
          agentContacts,
          agentPermissions
        );
      });

      connect.core.onAuthorizeSuccess(() => {
        console.log("ooo", "authorization succeeded! Hooray");
      });
      connect.core.onAuthFail(() => {
        console.log("ooo", "authorization failure! Hooray");
      });

      // On inbound communication
      connect.contact((contact) => {
        // receive contact metadata
        const contactAttributes = contact.getAttributes();
        console.log(
          "******",
          "Subscribe a method to be called for each newly detected agent contact."
        );
        console.log("ooo", "******", contact, contactAttributes);
      });
    },
  },
};
</script>

<style>
#the-canvas {
  border: 1px solid black;
  direction: ltr;
}

.d-none {
  display: none;
}

.fakeRect {
  position: absolute;
  background: transparent;
  height: 100px;
  width: 100px;
  left: 359px;
  bottom: 150px;
  z-index: 1;
  content: "";
}

canvas {
  position: relative;
}
</style>
