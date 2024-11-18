## TODO
- [ ] Open in CW1 button
- [ ] Info on hover
- [ ] The tags below the table
- [ ] Captions


```
          <template #[`item.Actions`]="{ item }">
            <div class="d-flex flex-row align-center">
              <wtg-checkbox v-if="isOptionalCharge(item)">s-icon-checkbox-mark</wtg-checkbox>
              <!-- TODO(URS): Bind this to a selected state. -->
              <wtg-icon v-if="!isOptionalCharge(item)">s-icon-checkbox-mark</wtg-icon>

              <wtg-menu v-if="item.CalculatorCode === Calculators.CMB.Code">
                <template #activator="{ on }">
                  <wtg-icon-button icon="s-icon-menu-meatballs" :variant="WtgBtnVariantType.Ghost" v-on="on" />
                </template>
                <wtg-list>
                  <wtg-list-item @click="() => showCmbCalculatorModal(item.SourcePK)">
                    {{ captions.viewCmbCalculator }}
                  </wtg-list-item>
                </wtg-list>
              </wtg-menu>
            </div>
          </template>
```