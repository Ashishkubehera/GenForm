<template>
  <div>
    <v-card class="mx-auto">
      <v-card-title>Result 😎</v-card-title>
      <v-snackbar
        top
        right
        color="cyan darken-2"
        :timeout="3000"
        v-model="Snackbar"
      >
        {{ SnackbarMsg }}
        <v-btn text @click="Snackbar = false">
          Close
        </v-btn>
      </v-snackbar>
      <v-form lazy-validation v-model="Valid" ref="Form">
        <v-container class="text-center">
          <v-flex v-for="(Field, index) in Fields" :key="index">
            <v-text-field
              v-if="Field.Type === 'TextField' && Field.Rules !== 'Password'"
              :dense="FormSettings.Dense === true"
              :filled="FormSettings.InputStyle === 'Filled' ? true : false"
              :outlined="FormSettings.InputStyle === 'Outlined' ? true : false"
              :solo="FormSettings.InputStyle === 'Solo' ? true : false"
              v-model="Field.DefaultVal"
              :label="Field.Label"
              :type="Field.Rules"
              :min="0"
              :counter="Field.Counter"
              :rules="Rules(Field)"
            ></v-text-field>

            <v-text-field
              v-if="Field.Type === 'TextField' && Field.Rules === 'Password'"
              :dense="FormSettings.Dense === true"
              :filled="FormSettings.InputStyle === 'Filled' ? true : false"
              :outlined="FormSettings.InputStyle === 'Outlined' ? true : false"
              :solo="FormSettings.InputStyle === 'Solo' ? true : false"
              v-model="Field.DefaultVal"
              :append-icon="Field.TogglePassword ? 'mdi-eye' : 'mdi-eye-off'"
              :type="Field.TogglePassword ? 'text' : 'password'"
              :label="Field.Label"
              hint="At least 8 characters"
              :counter="Field.Counter"
              :rules="Rules(Field)"
              @click:append="Field.TogglePassword = !Field.TogglePassword"
            ></v-text-field>

            <v-textarea
              v-if="Field.Type == 'Textarea'"
              :filled="Field.Rules === 'Filled' ? true : false"
              :outlined="Field.Rules === 'Outlined' ? true : false"
              :solo="Field.Rules === 'Solo' ? true : false"
              :min="0"
              :label="Field.Label"
              :counter="Field.Counter"
              :value="Field.DefaultVal"
              :rules="Rules(Field)"
            ></v-textarea>
          </v-flex>

          <v-btn
            v-if="Fields.length > 0"
            :disabled="!Valid"
            color="success"
            class="mr-4"
            @click="Validate"
            >Validate</v-btn
          >

          <v-btn
            v-if="Fields.length > 0"
            color="error"
            class="mr-4"
            @click="Reset"
            >Reset Form</v-btn
          >
        </v-container>
      </v-form>
      <v-card-actions>
        <v-btn v-if="Fields.length > 0" @click="Clipboard" text>
          Give Me Code
          <v-icon>mdi-code-tags-check</v-icon>
        </v-btn>
      </v-card-actions>
    </v-card>
    <pre
      ref="Syntax"
      class="CodeBackground"
    ><span ref="CodeSyntax1">&lt;template&gt;
    &lt;v-form lazy-validation v-model="Valid" ref=&quot;Form&quot;&gt; 
        &lt;v-container class=&quot;text-center&quot;&gt;</span>
        <span v-for="(Field, index) in Fields" :key="index"> 
          <span ref="CodeSyntax2" v-if="Field.Type === 'TextField' && Field.Rules !== 'Password'">&lt;v-text-field
              {{FormSettings.Dense === true ? 'dense' : null}}
              {{FormSettings.InputStyle === 'Filled' ? 'filled' : null}}{{FormSettings.InputStyle === 'Solo' ? 'solo' : null}}{{FormSettings.InputStyle === 'Outlined' ? 'outlined' : null}}
              label="{{Field.Label}}"
              {{Field.Rules && 'type="'+Field.Rules+'"'}}
              :min="0"
              :counter="{{Field.Counter}}"
              v-model="Field_{{index+1}}"
              :rules="Rule_{{index+1}}"
          &gt;&lt;/v-text-field&gt;</span><span v-if="Field.Type === 'TextField' && Field.Rules === 'Password'">&lt;v-text-field
              {{FormSettings.Dense === true ? 'dense' : null}}
              {{FormSettings.InputStyle === 'Filled' ? 'filled' : null}}{{FormSettings.InputStyle === 'Solo' ? 'solo' : null}}{{FormSettings.InputStyle === 'Outlined' ? 'outlined' : null}}
              label="{{Field.Label}}"
              hint="At least 8 characters"
              :append-icon="TogglePass_{{index+1}} ? 'mdi-eye' : 'mdi-eye-off'"
              :type="TogglePass_{{index+1}} ? 'text' : 'password'"
              :min="0" 
              :counter="{{Field.Counter}}"
              v-model="Field_{{index+1}}"
              :rules="Rule_{{index+1}}"
              @click:append="TogglePass_{{index+1}} = !TogglePass_{{index+1}}"
          &gt;&lt;/v-text-field&gt;</span>
          <span v-if="Field.Type === 'Textarea'">&lt;v-textarea
              {{Field.Rules === 'Filled' ? 'filled' : null}}{{Field.Rules === 'Solo' ? 'solo' : null}}{{Field.Rules === 'Outlined' ? 'outlined' : null}}
              label="{{Field.Label}}"
              :min="0"
              :counter="{{Field.Counter}}"
              :value="Field_{{index+1}}"
              :rules="Rule_{{index+1}}"
          &gt;&lt;/v-textarea&gt;</span></span>
        &lt;/v-container&gt;
    &lt;/v-form&gt;
&lt;/template&gt;

&lt;script&gt;
  export default { 
    data: () =&gt; ({
      Valid: true,
      <span v-for="(Field, ModalIndex) in Fields" :key="ModalIndex + 'Modal'">Field_{{ModalIndex+1}}: '',{{ModalIndex !== Fields.length -1 ? '\n&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;' : null }}</span>
      <span v-for="(Field, RulesIndex) in Fields" :key="RulesIndex + 'Rules'">Rule_{{RulesIndex+1}}: [{{Field.FieldRequired === true ? 'v => !!v || "' + Fields[RulesIndex].Label + ' is required",' : null}} {{Field.Counter > 0 ? 'v => v.length= '+Fields[RulesIndex].Counter+ ' || "Max ' +Fields[RulesIndex].Counter+ ' characters",' : null }} {{Field.Rules === 'Email' ? `v => /^(([^()\\[\\]\\\\.,;:\\s@"]+(\\.[^()\\[\\]\\\\.,;:\\s@"]+)*)|(".+"))@((\\[[0-9]{1,3}\\.[0-9]{1,3}\\.[0-9]{1,3}\\.[0-9]{1,3}\\])|(([a-zA-Z\\-0-9]+\\.)+[a-zA-Z]{2,}))$/.test(v) || "Email must be valid",` : null}} {{Field.Rules === 'Password' ? 'v => v.length >= 8 || "Min 8 characters"' : null }}],{{RulesIndex !== Fields.length -1 ? '\n&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;' : null }}</span>
      <span v-for="(Field, PassToggleIndex) in Fields" :key="PassToggleIndex + 'PasswordProperty'"><span v-if="Field.Rules === 'Password'">TogglePass_{{PassToggleIndex+1}}: false{{PassToggleIndex  !== Fields.length - 1 ? ',\n&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;' : null }}</span></span>
    })
  }
&lt;/script&gt;</pre>
    <v-dialog v-model="Dialog" max-width="700">
      <v-card shaped>
        <v-flex pl-5 pr-2 pt-3 class="headline"
          >GENFORM
          <v-btn
            icon
            href="https://github.com/Ashishkubehera/VuetiForm"
            target="_blank"
            rel="noopener noreferrer"
            ><v-icon>mdi-github</v-icon></v-btn
          >
          <v-btn
            @click="CopyToClipboard"
            class="float-right"
            color="success lighten-1"
            text
          >
            Copy to Clipboard <v-icon>mdi-content-copy</v-icon>
          </v-btn>
        </v-flex>
        <v-card-text>
          <PrismEditor
            class="NoBoxShadow"
            readonly
            :code="SourceCode"
            language="vue"
          ></PrismEditor>
        </v-card-text>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
import PrismEditor from "vue-prism-editor";
export default {
  components: {
    PrismEditor
  },

  data: () => ({
    Snackbar: false,
    SnackbarMsg: "Copied to Clipboard",
    SourceCode: "",
    Dialog: false,
    Valid: true,
    Fields: [],
    FormSettings: null
  }),

  methods: {
    Validate() {
      if (this.$refs.Form.validate()) {
        this.snackbar = true;
      }
    },

    Reset() {
      this.$refs.Form.reset();
    },

    Rules(Field) {
      let RulesArray = [];

      if (Field.FieldRequired) {
        RulesArray.push(v => !!v || Field.Label + " is required");
      }

      if (Field.Counter > 0) {
        if (Field.DefaultVal) {
          RulesArray.push(
            v =>
              v.length <= Field.Counter ||
              "Max " + Field.Counter + " characters"
          );
        }
      }

      if (Field.Rules == "Email") {
        RulesArray.push(
          v =>
            /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/.test(
              v
            ) || "E-mail must be valid"
        );
      }

      if (Field.Rules == "Password") {
        if (Field.DefaultVal) {
          RulesArray.push(v => v.length >= 8 || "Min 8 characters");
        }
      }
      return RulesArray;
    },

    CodeSyntaxRules(Field) {
      let RulesArray = [];

      if (Field.FieldRequired) {
        RulesArray.push('v => !!v || "Field is required"');
      }

      if (Field.Counter > 0) {
        RulesArray.push(
          `v => v.length <= Field.Counter || "Max characters"
        `
        );
      }
      return RulesArray.toString();
    },
    Clipboard() {
      this.Dialog = true;

      let Ref = this.$refs.Syntax.innerHTML;

      this.SourceCode = this.DecodeHTMLEntities(Ref);
    },
    DecodeHTMLEntities(text) {
      var entities = [
        ["amp", "&"],
        ["apos", "'"],
        ["#x27", "'"],
        ["#x2F", "/"],
        ["#39", "'"],
        ["#47", "/"],
        ["lt", "<"],
        ["gt", ">"],
        ["nbsp", " "],
        ["quot", '"']
      ];

      for (var i = 0, max = entities.length; i < max; ++i)
        text = text.replace(
          new RegExp("&" + entities[i][0] + ";", "g"),
          entities[i][1]
        );
      return text
        .replace(/<\/?span[^>]*>/g, "")
        .replace(/<!--.*?-->/g, "")
        .replace(/^\s*[\r\n]/gm, "");
    },
    CopyToClipboard() {
      this.$clipboard(this.SourceCode);
      this.Snackbar = true;
    }
  },

  mounted() {
    this.$root.$on("TextField", data => {
      this.Fields = data;
    });

    this.$root.$on("FormSettings", data => {
      this.FormSettings = data;
    });
  }
};
</script>

<style>
.CodeBackground {
  padding: 5px;
  background-color: #282c33;
  max-width: 650px;
  overflow: auto;
  border-radius: 5px;
  display: none;
}

code {
  box-shadow: none !important;
}
</style>
