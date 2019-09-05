# Card Navigation

## Description
- Made with Card Layout
- Only available from 19.3

## Preview
![alt text][cardNavigationPreview]

## Parameters
Name      | Description
--- | ---
`local!iconColor` | Navigation Item Icon Color
`local!iconSize` | Navigation Item Icon Size
`local!titleColor` | Navigation Item Title Color
`local!descriptionColor` | Navigation Item Description Color
`local!descriptionColorSelected` | Navigation Item Description Color (selected state)
`local!itemBgColor` | Navigation Item Background Color
`local!itemBgColorSelected` | Navigation Item Background Color (selected state)
`local!itemPerRow` | Number of items to display par row


## Code
```
a!localVariables(
  /** UI Parameters **/
  local!iconColor: "#EA7773",
  local!iconSize: "MEDIUM_PLUS",
  local!titleColor: "#EA7773",
  local!descriptionColor: "#333945",
  local!descriptionColorSelected: "#fff",
  local!itemBgColor: "#EAF0F1",
  local!itemBgColorSelected: "#616C6F",
  local!itemPerRow: 4,
  /* *****  */
  /** Content Parameters **/
  local!refDataTableNames: {
    {
      icon: "calendar",
      title: "Appian Events",
      description: "Event's organized by Appian for Customers"
    },
    {
      icon: "users",
      title: "Corporate User Repository",
      description: "Internal Employees Repository"
    },
    {
      icon: "gift",
      title: "Features",
      description: "Appian product features developed by Engineering"
    },
    {
      icon: "truck",
      title: "Appian Software Releases",
      description: "Appian's Software Releases"
    },
    {
      icon: "handshake-o",
      title: "Partners Organizations",
      description: "Appian's active partners"
    },
    {
      icon: "bullseye",
      title: "Engineering Annual goals",
      description: null
    }
  },
  /* *****  */
  /** Internal Parameters (DO NOT TOUCH) **/
  local!totalItems: count(
    local!refDataTableNames
  ),
  local!rows: quotient(
    local!totalItems,
    local!itemPerRow
  ),
  local!remainder: mod(
    local!totalItems,
    local!itemPerRow
  ),
  local!totalRows: if(
    local!remainder = 0,
    local!rows,
    local!rows + 1
  ),
  /* *****  */
  /** Dynamic Parameters **/
  local!selectedIndex: 1,
  /* *****  */
  {
    a!columnsLayout(
      columns: {
        a!columnLayout(
          contents: {
            a!forEach(
              items: repeat(
                local!totalRows,
                null
              ),
              expression: with(
                local!colIndex: fv!index,
                a!columnsLayout(
                  columns: {
                    a!forEach(
                      items: repeat(
                        local!itemPerRow,
                        null
                      ),
                      expression: a!localVariables(
                        local!itemIndex: (
                          local!colIndex - 1
                        ) * local!itemPerRow + fv!index,
                        local!item: index(
                          local!refDataTableNames,
                          local!itemIndex,
                          null
                        ),
                        a!columnLayout(
                          contents: {
                            a!cardLayout(
                              showBorder: false,
                              showWhen: not(
                                isnull(
                                  local!item.title
                                )
                              ),
                              contents: {
                                a!richTextDisplayField(
                                  labelPosition: "COLLAPSED",
                                  value: {
                                    a!richTextIcon(
                                      icon: local!item.icon,
                                      size: local!iconSize,
                                      color: local!iconColor
                                    ),
                                    char(
                                      10
                                    ),
                                    a!richTextItem(
                                      text: local!item.title,
                                      color: local!titleColor,
                                      style: "STRONG"
                                    ),
                                    char(
                                      10
                                    ),
                                    a!richTextItem(
                                      text: local!item.description,
                                      color: if(
                                        tointeger(
                                          local!selectedIndex
                                        ) = tointeger(
                                          local!itemIndex
                                        ),
                                        local!descriptionColorSelected,
                                        local!descriptionColor
                                      ),
                                      size: "SMALL",
                                      style: "EMPHASIS"
                                    )
                                  },
                                  align: "CENTER"
                                )
                              },
                              link: a!submitLink(
                                value: local!itemIndex,
                                saveInto: local!selectedIndex
                              ),
                              style: if(
                                tointeger(
                                  local!selectedIndex
                                ) = tointeger(
                                  local!itemIndex
                                ),
                                local!itemBgColorSelected,
                                local!itemBgColor
                              ),
                              height: "SHORT"
                            )
                          }
                        )
                      )
                    )
                  }
                )
              )
            )
          }
        )
      }
    )
  }
)
```


[cardNavigationPreview]: https://github.com/ps-france/ui-sail-snippets/raw/master/screenshots/card_navigation.PNG "Card Navigation"