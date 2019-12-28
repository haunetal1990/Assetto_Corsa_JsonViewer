Imports System.IO</br>
Imports Newtonsoft.Json</br>
Public Class Form1</br>

    Private Sub Form1_Load(sender As Object, e As EventArgs) Handles MyBase.Load
        ListView1.Items.Clear()

        If (TextBox1.Text.Length > 0) Then
            Dim sfile As String = Nothing
            Dim sPath As String = TextBox1.Text
            For Each sfile In My.Computer.FileSystem.GetFiles(sPath, FileIO.SearchOption.SearchAllSubDirectories, "ui_car.json")
                For Each Zeile As String In System.IO.File.ReadLines(sfile)
                    If Zeile.Trim.StartsWith(Chr(34) & "name" & Chr(34)) Then
                        ListView1.Items.Add(Zeile)

                    ElseIf Zeile.Trim.StartsWith(Chr(34) & "version" & Chr(34)) Then
                        ListView1.Items.Add(Zeile)
                    End If
                Next
            Next
            For Each LVI As ListViewItem In ListView1.Items
                LVI.Text = LVI.Text.Replace(Space(2) & Chr(34) & "name" & Chr(34) & ":" & Space(1), "")
                LVI.Text = LVI.Text.Replace(Chr(34), "")
                LVI.Text = LVI.Text.Replace(",", "")
            Next
        Else
            MessageBox.Show("Datei nicht gefunden")
        End If
    End Sub

' bis hier hin ging es darum, per Listview anzuzeigen, wie viele ui_car.json in den jeweiligen Unterordnern (link in Textbox) zu finden sind.
diese sollten dann in die Listview angezeigt werden. Ein weiterer Gedanke war, mehr Informationen heraus zu holen. Bis ich dann gemerkt hatte,
dass das per Json doch viel einfacher geht.

    Private Sub Button1_Click(sender As Object, e As EventArgs) Handles Button1.Click
        Dim result As preferenceModel = JsonConvert.DeserializeObject(Of preferenceModel)(RichTextBox1.Text)
        With ListView1.Items.Add(result.Name)
            .SubItems.Add(result.brand)
            .SubItems.Add(result.description) 'usw
        End With
    End Sub
End Class

' zusätzlich muss man per Projekt / Komponente hinzufügen / Codedatei eine preferenceModel.vb hinzufügen. Diese beeinhaltet folgendes:

Public Class preferenceModel</br>
    Public Name As String</br>
    Public brand As String</br>
    Public description As String</br>
End Class</br>
