<template>
  <div class="q-pa-md q-gutter-md">
    <div class="row justify-center">

      <q-img src="https://cdn.quasar-framework.org/img/material.png" style="height: 100px">
        <q-menu
          touch-position
          context-menu
        >

          <q-list dense style="min-width: 100px">
            <q-item clickable v-close-menu>
              <q-item-section>Open...</q-item-section>
            </q-item>
            <q-item clickable v-close-menu>
              <q-item-section>New</q-item-section>
            </q-item>
            <q-separator />
            <q-item clickable>
              <q-item-section>Preferences</q-item-section>
              <q-item-section side>
                <q-icon name="keyboard_arrow_right" />
              </q-item-section>

              <q-menu anchor="top right" self="top left">
                <q-list>
                    <q-item
                      v-for="n in 3"
                      :key="n"
                      clickable
                    >
                      <q-item-section>Submenu Label</q-item-section>
                      <q-item-section side>
                        <q-icon name="keyboard_arrow_right" />
                      </q-item-section>
                      <q-menu auto-close anchor="top right" self="top left">
                        <q-list>
                          <q-item
                            v-for="n in 3"
                            :key="n"
                            clickable
                          >
                            <q-item-section>3rd level Label</q-item-section>
                          </q-item>
                        </q-list>
                      </q-menu>
                    </q-item>

                </q-list>
              </q-menu>

            </q-item>
            <q-separator />
            <q-item clickable v-close-menu>
              <q-item-section>Quit</q-item-section>
            </q-item>
          </q-list>

        </q-menu>
      </q-img>
    </div>
  </div>
</template>
