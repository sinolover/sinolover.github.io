<h1>将Font对像转化为字符串存储</h1>

提交日期:2003-8-7  

看看下面的代码：
~~~delphi
function FontToString(Font: TFont): string;

begin

    with Font do

    Result := Format('%.8x%.8x%.4x%.4x%.1x%.2x%.1x%.4x%s', [Color, Height, Size,

    PixelsPerInch, Byte(Pitch), CharSet, Byte(Style), Length(Name), Name]);

end;

procedure StringToFont(Str: string; Font: TFont);

var

    Buff: string;

begin

    if Length(Str) < 33 then raise Exception.Create('Error Font Format String');

    Buff := Copy(Str, 1, 8);

    Font.Color := StrToInt('$' + Buff);

    Buff := Copy(Str, 9, 8);

    Font.Height := StrToInt('$' + Buff);

    Buff := Copy(Str, 17, 4);

    Font.Size := StrToInt('$' + Buff);

    Buff := Copy(Str, 21, 4);

    Font.PixelsPerInch := StrToInt('$' + Buff);

    Buff := Copy(Str, 25, 1);

    Font.Pitch := TFontPitch(StrToInt('$' + Buff));

    Buff := Copy(Str, 26, 2);

    Font.Charset := TFontCharSet(StrToInt('$' + Buff));

    Buff := Copy(Str, 28, 1);

    Font.Style := TFontStyles(Byte(StrToInt('$' + Buff)));

    Buff := Copy(Str, 29, 4);

    Font.Name := Copy(Str, 33, StrToInt('$' + Buff));

end;

procedure TForm1.Button1Click(Sender: TObject);

begin

    Memo1.Text := FontToString(Memo1.Font);

end;

procedure TForm1.Button2Click(Sender: TObject);

begin

    if FontDialog1.Execute then

        Memo1.Font := FontDialog1.Font;

end;

procedure TForm1.Button3Click(Sender: TObject);

begin

    StringToFont(Memo1.Text, Memo1.Font);

end;
 
~~~
 
