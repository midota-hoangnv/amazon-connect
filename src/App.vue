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
    // this.init();
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
        "beffd482-e513-4b2b-aac5-aa67e6ca3314"
      );
      amazon_connect("styles", {
        iconType: "VOICE",
        openChat: { color: "#ffffff", backgroundColor: "#123456" },
        closeChat: { color: "#ffffff", backgroundColor: "#123456" },
      });
      amazon_connect(
        "snippetId",
        "QVFJREFIakNnb2I4UUFhMXdNbmZJdnEvYXJSclVQTEk0eUtlQjYvUkZmQ2Q5NU1hSWdGUnYvRmp3MGREZnp4R0V5UDVLVlFhQUFBQWJqQnNCZ2txaGtpRzl3MEJCd2FnWHpCZEFnRUFNRmdHQ1NxR1NJYjNEUUVIQVRBZUJnbGdoa2dCWlFNRUFTNHdFUVFNM2VQQzhVU3lJdys4RnhXYkFnRVFnQ3NWZ2ovOXZDZjEzcG9GbGI0NDVxNWd3Q0hqcmVIaHYrNEIyMUJUdzVMa1FBbXVycFRQQ1VBRCs5d0w6Om5wVmxZQmh3VFRlQjkzb1MrWkx1ZzFlMTMwUDhSZVYwRHZJbVBpdVVHWTBKVWYzZXd0aGREMWFQa2xaM2tNQWx2ZXpDOTFXMVhOaHRIeHZIbS9vV0JvMlFMdHBMb1JTMVRiZkJpNlptY0VLMDBQSkhBWTFvN1FCSGY1amV4MVVMd0dGMkdHUmFXcjZQTGVQSE5nT1FuNmlYd1AyOHB6az0="
      );

      // (function(w, d, x, id) {
      //   var s = d.createElement("script");
      //   s.src =
      //     "https://d1nv5i00u1m742.cloudfront.net/amazon-connect-chat-interface-client.js";
      //   s.async = 1;
      //   s.id = id;
      //   d.getElementsByTagName("head")[0].appendChild(s);
      //   w[x] =
      //     w[x] ||
      //     function() {
      //       (w[x].ac = w[x].ac || []).push(arguments);
      //     };
      // })(
      //   window,
      //   document,
      //   "amazon_connect",
      //   "beffd482-e513-4b2b-aac5-aa67e6ca3314"
      // );
      // amazon_connect("styles", {
      //   iconType: "VOICE",
      //   openChat: { color: "#ffffff", backgroundColor: "#123456" },
      //   closeChat: { color: "#ffffff", backgroundColor: "#123456" },
      // });
      // amazon_connect(
      //   "snippetId",
      //   "QVFJREFIakNnb2I4UUFhMXdNbmZJdnEvYXJSclVQTEk0eUtlQjYvUkZmQ2Q5NU1hSWdGUnYvRmp3MGREZnp4R0V5UDVLVlFhQUFBQWJqQnNCZ2txaGtpRzl3MEJCd2FnWHpCZEFnRUFNRmdHQ1NxR1NJYjNEUUVIQVRBZUJnbGdoa2dCWlFNRUFTNHdFUVFNM2VQQzhVU3lJdys4RnhXYkFnRVFnQ3NWZ2ovOXZDZjEzcG9GbGI0NDVxNWd3Q0hqcmVIaHYrNEIyMUJUdzVMa1FBbXVycFRQQ1VBRCs5d0w6Om5wVmxZQmh3VFRlQjkzb1MrWkx1ZzFlMTMwUDhSZVYwRHZJbVBpdVVHWTBKVWYzZXd0aGREMWFQa2xaM2tNQWx2ZXpDOTFXMVhOaHRIeHZIbS9vV0JvMlFMdHBMb1JTMVRiZkJpNlptY0VLMDBQSkhBWTFvN1FCSGY1amV4MVVMd0dGMkdHUmFXcjZQTGVQSE5nT1FuNmlYd1AyOHB6az0="
      // );
      // amazon_connect("supportedMessagingContentTypes", [
      //   "text/plain",
      //   "text/markdown",
      // ]);
      amazon_connect("customLaunchBehavior", {
        skipIconButtonAndAutoLaunch: true,
        alwaysHideWidgetButton: true,
      });
      amazon_connect("enableLogs", true);
      amazon_connect("authenticate", function(callback) {
        console.log("testtt");
        callback(
          "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJiZWZmZDQ4Mi1lNTEzLTRiMmItYWFjNS1hYTY3ZTZjYTMzMTQiLCJhdHRyaWJ1dGVzIjp7InNlc3Npb25JZCI6IjAxMjM0NTY3Nzg5In0sImlhdCI6MTcxMjEyNDA2OCwiZXhwIjoxNzEyMTI0Njg1fQ.8HZeuYrSmEJKIHdt7KHSaOjEzOPCIgum805nGjxUodQ"
        );
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
