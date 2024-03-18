# XQuery Notes

## List files in a collection modified after a certain date

    let $fullCollection := xmldb:xcollection("/db/datasets/myapp/data")
    let $filteredCollection := xmldb:find-last-modfiied-since($fullCollection, xs:dateTime("2021-11-23T21:13:00"))
    return $filteredCollection

## List filename and specific element from files in a collection

    let $fullCollection := xmldb:xcollection("/db/datasets/myapp/data")
    return
        <table>
        {
            for $item in $fullCollection
                return
                    <tr>
                    {
                        concat("<td>", util:document-name($item), "</td><td>", data($item/starship/captain), "</td>")
                    }
                    </tr>
        }
        </table>

## Count of the files in a collection

    let $path := '/db/datasets/myapp/data'
    return count(xmldb:xcollection($path))

## Deleting all the files in a collection

    let $path := '/db/datasets/myapp/data'
    for $a in xmldb:xcollection($path)
        return xmldb:remove($path, util:document-name($a))

## Casting string to integer

    xquery version "3.1";

    let $originalValue := <dude>1234567890</dude>
    let $castValue := xs:integer($originalValue)

    let $testWithout := format-number($originalValue, '0000000')
    let $testWith := format-number($castValue, '0000000')

    return (
        $testWithout,
        $testWith
    )

## Using substring

    xquery version "3.1";

    let $originalValue := <dude>1234567890</dude>
    let $testSubstring := substring($originalValue, 4, 3)

    return (
        $originalValue,
        $testSubstring
    )
